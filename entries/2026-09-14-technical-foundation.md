---
title: The Technical Foundation
date: 2026-09-14 12:00:00 +0200
tag: Engine
description: What Magic is built on — the stack, the precision model, the world chart, the frame, the data, and the process that keeps it honest.
---

Magic renders the Earth from any height. Full disc at 15,000 km, a ridge at eye height,
and every altitude between, with one camera, one world model and no mode switch. The
terrain is measured (Copernicus GLO-30, GEBCO), the sky is physical, and the clock drives
the sun, moon and stars. This post is the foundation everything else stands on: the stack,
the precision rules, the chart, the frame, the data, and the way we work. Terrain and
atmosphere get their own posts.

<figure>
  <img src="{{ '/assets/images/tf-full-disc.jpg' | relative_url }}" alt="Earth as a full disc from 15,000 km, Africa and Arabia under the sun">
  <figcaption>15,000 km, local noon. The same renderer, with the same terrain mesh, draws the next image.</figcaption>
</figure>

<figure>
  <img src="{{ '/assets/images/tf-ground.jpg' | relative_url }}" alt="A rocky slope at eye height, golden hour, from the same renderer">
  <figcaption>1.7 m above a slope on the 45 E meridian, golden hour. Catalogue view <code>G6</code>.</figcaption>
</figure>

## Stack

Rust 2021, Vulkan 1.3 through `ash`, `gpu-allocator`, `winit` for the window and `egui`
for the panel. Shaders are GLSL 460, compiled at startup by shaderc to SPIR-V 1.6 and
reloadable from source while the app runs. The target is an RTX 4090 at 2560 × 1440.

The device features we depend on are the ones that make a bindless, compute-first design
comfortable: `shaderFloat64` and `shaderInt64`, buffer device addresses, descriptor
indexing with update-after-bind, formatless storage images, timeline semaphores, scalar
block layout, dynamic rendering, synchronization2 and timestamp queries. We do not use
mesh shaders, ray queries, sparse residency or a second compute queue.

Every pass sees one descriptor set with two bindings — storage images and sampled
images, aliased per component type — and one push constant: the device address of this
frame's `FrameData` plus a pass parameter. Everything else (tile slot table, CBT pool,
atmosphere parameters, feedback, stats) is reached through `buffer_reference`.

```glsl
layout(set = 0, binding = 0) uniform image2D  gImages[];
layout(set = 0, binding = 1) uniform sampler2D gTextures[];

layout(push_constant) uniform PushConstants {
    FrameDataRef frame;   // address of this frame slot's FrameData
    uint passParam;
    uint pad;
} gPush;
#define FRAME gPush.frame.d
```

`FrameData` exists twice, once in Rust and once in GLSL. A test parses the GLSL struct and
checks every field offset against `std::mem::offset_of!`, and the renderer checks the
field count at startup. That check is not decorative: the first thing it did for this post
was refuse a stale binary with *"the shader declares 211 fields, this binary has 210"*.

## Precision

Earth's radius is 6.371 × 10⁶ m. An f32 at that magnitude has a ULP of 0.5 m, so a
planet-centred f32 position is useless for a camera standing on the ground. The rules:

- CPU world and camera state are f64, planet-centred. X points at longitude 0, Y is north.
- GPU work is camera-relative. The camera sits at the origin of the render frame.
- **The planet centre is never an f32 vector.** The camera is described by `(up, h)`:
  its geodetic up vector and its height above the datum, each split into a hi and lo
  f32 so the f64 residual survives the trip.

```rust
let up = cam.position / dist;
let up_hi = up.as_vec3();
let up_lo = (up - up_hi.as_dvec3()).as_vec3();
let altitude = dist - planet.radius;
let altitude_hi = altitude as f32;
let altitude_lo = (altitude - f64::from(altitude_hi)) as f32;
```

Every ray/shell intersection is written in that form. The discriminant of the usual
quadratic subtracts two numbers of size 10¹³; this one never does.

```glsl
// Ray d from a camera h above the datum, against a shell of radius shellR.
bool shellIntersect(vec3 d, float shellR, float h, out float tNear, out float tFar) {
    float hs   = h + (FRAME.radius - shellR);        // height above this shell
    float rh   = shellR + hs;                        // distance camera - centre
    float cosA = -dot(d, FRAME.upHi) - dot(d, FRAME.upLo);
    float sinA = sqrt(max(1.0 - cosA * cosA, 0.0));
    float p    = rh * sinA;                          // perpendicular distance centre - ray
    float rmp  = shellR * (cosA * cosA) / (1.0 + sinA) - hs * sinA;   // shellR - p, no cancellation
    float disc = rmp * (shellR + p);
    if (disc < 0.0) return false;
    float s = sqrt(disc);
    float b = rh * cosA;
    float c = hs * (2.0 * shellR + hs);              // = |o|^2 - shellR^2, formed without |o|^2
    tNear = c / (b + s);                             // stable near root
    tFar  = b + s;
    return tFar > 0.0;
}
```

The same trick gives the height above the datum at any distance along a ray, which the
atmosphere integrators use; and the horizon dip at height *h* is `asin(sqrt(h(2R+h))/(R+h))`,
never `acos(R/(R+h))`.

Depth is reverse-Z with an infinite far plane, `near / viewZ`, and the near plane follows
the height above the *displayed* ground rather than the datum altitude, so a camera on a
summit keeps its depth precision where the geometry is.

The one place per pixel we spend f64 on the GPU is the chart lookup of a surface point.
GLSL has no double-precision trigonometry, so there is a hand-rolled `atan64`: halve the
argument three times with `atan(y) = 2 atan(y / (1 + sqrt(1 + y²)))`, then a five-term
series. Error under 10⁻¹² rad, which is 6 µm on the surface.

## The chart

The world is a cube sphere. Six faces, each a chart `(u, v) ∈ [-1, 1]²`, mapped to a
direction by warping `(u, v)`, placing the point on the face's tangent plane and
normalising. The warp is the **tangent warp at π/4** — `x = tan(u·π/4) / tan(π/4)` —
which makes the angle linear in the chart coordinate along the face centre line. Its
worst corner anisotropy is √3. We measured six projections (gnomonic, two tangent
variants, Everitt, COBE, QSC) before fixing this one; it is part of the world definition
and cannot change once tiles exist, because every stored tile address and every
per-tile inverse polynomial assumes it.

Tiles are 512 samples a side with a four-texel apron (520 stored), addressed by
`(face, level, x, y)`. The texel size at a level is a closed form:

```rust
/// Metres per texel along the face centre line at `level`.
pub fn level_texel_size(radius: f64, level: u32) -> f64 {
    (radius * FRAC_PI_2) / (512.0 * (1u64 << level) as f64)
}
```

That gives 19.5 km at L0, 38 m at L9 (where the measured data stops) and 7.5 cm at L18.
A point on a cube edge belongs to two faces; it is always evaluated on the lower-numbered
one from both sides, so the two triangles meeting there get bit-identical positions.
Before that rule there was a crack at the seam exactly one rounding error wide, and a
ray through it went straight through the planet.

## The frame

The renderer is compute-first and deferred. The only rasterisation is the terrain mesh
(and, near the ground, ocean, trees and the canopy shell), and rasterising produces no
colour: the fragment stage writes a **hit record**. Shading, sky, atmosphere and post are
all compute.

```
hit0 = (t, height above datum, previous-height delta, level blend weight)   RGBA32F
hit1 = (slot, kind | flags | level << 8 | provenance << 16 | water << 24,
        texel.x bits, texel.y bits)                                          RGBA32UI
```

The recorded pass order, as `PlanetRenderer::render` lays it down:

1. **tiles** — this frame's tile uploads, then max-mip and normal derivation for them.
2. **atmosphere** — transmittance and multiple-scattering LUTs if parameters changed;
   sky-view and aerial-perspective froxels every frame.
3. **cbt_update** — the terrain mesh refines and coarsens (up to four iterations).
4. **cbt_draw** — a depth-only pass selects the nearest triangle, then an `EQUAL`-depth
   pass publishes the hit record. Depth ordering alone does not order storage writes.
5. **vegetation**, **canopy** — same pattern, into the same buffers.
6. **water_sky** — settles what the mesh left: the analytic ocean where the published
   height is below the datum, and the sky, which is shaded here.
7. ocean spectrum/FFT/surface, weather, cloud caches, cloud shadow, cloud columns.
8. **materials** — virtual-texture page composition and per-pixel material resolve.
9. **shade** — irradiance, terrain shadow ray, ocean, aerial perspective.
10. **pop** — the surface-change counters read back one frame later.
11. clouds (march, temporal, composite), **taa**, **exposure** and its same-frame guard,
    **bloom**, resolve, UI.

Two frames are in flight on timeline semaphores. Each pass sits between GPU timestamps,
and the per-pass scopes are the columns of the telemetry CSV; a whole-frame timestamp
pair is reported next to their sum so the unexplained remainder is a number rather than
an assumption.

<figure>
  <img src="{{ '/assets/images/tf-wireframe.jpg' | relative_url }}" alt="The terrain mesh drawn as wireframe over the shaded image, near Mount Shasta">
  <figcaption>The terrain mesh as lines over the shaded frame (<code>--near-wireframe</code>), 2.6 km up near Mount Shasta. One concurrent binary tree, refined to a half-pixel residual.</figcaption>
</figure>

## Data

- **Elevation**: Copernicus GLO-30 (and GLO-90 where that is all there is), converted to
  a block pyramid on disk from L2 to L9. A block is 2048 texels square, exactly 4 × 4
  tiles, and a block texel *is* a tile texel — indexed, never resampled. GLO-30's 31 m
  lands on L9's 38 m.
- **Bathymetry**: GEBCO, stored through L6 (463 m onto a 305 m texel) and interpolated
  above. The DEM's 30 m coastline is in charge where the two meet.
- **Climate**: a WorldClim-derived map — annual temperature, monthly temperature spread,
  effective precipitation, mean height, wind — read by materials, snow, haze humidity and
  the weather field.
- **Colour**: NASA Blue Marble, one packed BC1 cube per month, 20,480 texels a face,
  selected by the simulation month. The *plain* edition, without NASA's baked hillshade,
  because we light the surface from our own normals.

Everything above L9 is a fixed function of the L9 tile. No generated level is ever the
input to another; that is the subject of the terrain post.

<figure>
  <img src="{{ '/assets/images/tf-level-tint.jpg' | relative_url }}" alt="Terrain coloured by resident tile level, from 10 km over Lassen Peak">
  <figcaption>Terrain tinted by resident tile level (<code>--level-tint</code>) from 10 km. A level boundary is a ring at constant ray distance.</figcaption>
</figure>

## Where it lands

Measured on the 4090 at 1440p with atmosphere, shadows, materials, TAA and the panel on,
current as of 2026-09-11: an orbit-to-ground descent with a landing hold runs at a frame
p99 of about 13 ms; a ground hold at eye height about 12 ms. The terrain mesh holds a few
thousand triangles at orbit, about 54,000 at 400 km and about 1.75 million at eye height,
in a 2²²-entry pool (673 MiB). The tile cache is 1,024 slots (3.2 GB, fixed at startup);
the whole renderer sits at about 4.25 GiB of device memory.

## How we work

Two people, distinct jobs. Kaliffen owns the project: direction, scope, and the
judgement on how something looks. Astra designs and writes the code. A short process
document binds both, and the rules that matter most for the engineering are these:

- **Design and task are one mapping.** A design section no task points at is not being
  built. A task with no design section is not defined.
- **Code owns its definitions.** Sizes, strides, offsets and schemas are read from source,
  never copied from a document. (Every number in this post was checked that way.)
- **Experiments run through the harness.** A measurement is a scenario file plus an
  existing verb — `run`, `ladder`, `compare`, `baseline`, `gate`, `images`, `hits`. The
  harness is Rust and shares the engine's types; Python may render a report but never
  produces a verdict. The engine writes `run.json` beside every `frames.csv`: commit,
  dirty flag, source and shader hashes, GPU, driver, resolved settings.
- **A comparison needs three runs a side**, or it is inconclusive. Quantiles are not
  additive; per-pass p99s are never summed into a stack p99.
- **Settle is a named operation**: zero pending source jobs, zero uploads, zero fine-detail
  gain, held for a declared number of frames — and it reports which criterion blocked if
  it did not converge.

The catalogue of views is a Rust table, not a folder of JSON. `--golden G6` places the
camera on the steepest slope within 5 km of a named anchor at golden hour, with the sun
computed from the astronomy model for that site on 2000-06-21. Every image on this site
is one of those ids or a camera file the same code wrote, and the caption names it.

That is the foundation. Next: the terrain — one mesh from orbit to contact.
