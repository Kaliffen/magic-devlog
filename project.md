---
layout: default
title: Project
permalink: /project/
---

# What is Magic?

Magic is a planet-scale Vulkan renderer, written in Rust, that puts you
anywhere between full-disc orbit and standing on the ground — on real,
measured terrain.

It's built on a single world model rather than separate "space" and "ground"
modes: the same renderer that draws Earth as a lit disc from 15,000 km also
puts a camera on a mountainside at 1.7 m, with no seam or mode-switch between
the two. Terrain comes from measured elevation data (DEM), draped with a
ground-to-orbit raster mesh; a physical atmosphere model handles scattering
and aerial perspective at every altitude; ground materials and seasonal snow
respond to season and latitude; day, night and moonlight are all placed by an
astronomy model from a single clock, so time of day is just a camera
setting.

Current focus: continuous flight from orbit to ground still has rough edges
— terrain isn't always ready in time, and frame budget slips under load. The
priority right now is making that descent pop-free at any speed.

## Who's building it

<div class="people">
  <div class="card">
    <div class="person-role">Owner · creative producer</div>
    <p><strong>Kaliffen</strong> sets direction, makes the creative and
    scope calls, and decides what "done" looks like for a given push.</p>
  </div>
  <div class="card">
    <div class="person-role">Design · code</div>
    <p><strong>Astra</strong> designs and builds the renderer — from the
    Vulkan pipeline up to the terrain, atmosphere and lighting models.</p>
  </div>
</div>

This is a learning project first: an excuse to go deep on real-time
rendering, planetary-scale data and the engineering problems that only show
up once "the whole Earth" is the level. It's not a vehicle for opinions on
anything outside that — the devlog sticks to the render.
