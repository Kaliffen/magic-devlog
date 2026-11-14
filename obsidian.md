---
layout: default
title: Obsidian
permalink: /obsidian/
description: Obsidian makes imaginary Earth-sized planets from a seed and renders them from orbit down to standing on the ground. Flights over planet 28.
---

<div class="hero">
  <h1>Obsidian — imaginary planets from a seed</h1>
  <p class="lede">
    Obsidian is a second project on this log. Where Magic renders the real Earth from
    measured elevation data, Obsidian makes up planets of its own: give it a seed and in
    about ten seconds it builds an Earth-sized world, then lets you fly from orbit down to
    standing in the grass on it. No geodata anywhere.
  </p>
</div>

## A planet built one cause after another

The generator does not paint terrain with noise and call it a planet. It builds one the
way planets are built, each step feeding the next:

1. **Plates.** Tectonic plates drift, rift, subduct and collide for a couple of hundred
   million years. That decides where the continents are and why the mountains are where
   they are. The history is kept, so the app can show you the youngest collision or the
   oldest rift.
2. **Terrain.** The crust the plates leave behind is dressed into shelves, trenches and a
   sea level.
3. **Erosion.** Rain is routed to the sea and cuts valleys on the way, coarse to fine.
4. **Climate.** Sun, wind and sea decide where it is warm and where it rains, and the
   rivers are cut again with the rain that actually falls.
5. **Surface.** Lakes fill by water balance, the ground takes its colour from climate and
   rock, and a detail cascade carries the ten-kilometre chart down to the scale of a
   footstep.

Every planet on this page is **seed 28**: 32 % land, 18 plates, 163 lakes, its highest
summit at 8,364 m, built in about nine seconds.

## The renderer

Obsidian is written in Rust on Vulkan, with the same ambition as Magic: one camera, one
world, no seam between the disc of the planet and a walk on it. Terrain is cut into tiles
that the generator makes on demand, on worker threads, as the camera asks for them. Near
the ground a concurrent binary tree refines the mesh to the pixel. Over it go a physical
atmosphere, volumetric clouds driven by the planet's own weather, an ocean, trees, shrubs
and grass scattered from a single vegetation function of the world, and a sky of real stars
and a real Moon, all placed by one clock.

## How these videos were made

Each flight is a route: a handful of keyframes with a place, a height above the ground, a
heading and a pitch, and the camera passes smoothly through them. The height is measured
from the ground smoothed along the path, more the higher you fly, so a low pass follows a
valley floor and a high one glides over the ridges instead of bobbing on each.

The frames are rendered offline at 2560&times;1440 and 30 fps, and a frame is only kept
once the terrain it shows has arrived and the mesh has stopped refining, so there is no
pop-in however slowly the tiles stream. They are then scaled to 1080p and encoded in two
passes to fit just under 10 MB each. Nothing is edited after the render.

## Orbit to the riverbank

One continuous flight cut into three parts that play one after another: a fall from
400 km onto a snow-capped massif, a sweep west across the ridges into a gorge, and the
river at low level until the camera sets down on the valley floor.

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-descent-1-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/descent-1.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/descent-1.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption><strong>1 of 3.</strong> From 400 km straight down onto the coast and the
  snowfields, levelling out at 4.5 km over the massif.
  <a href="{{ '/assets/video/obsidian/descent-1.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-descent-2-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/descent-2.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/descent-2.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption><strong>2 of 3.</strong> West across the ridges and down into the gorge.
  <a href="{{ '/assets/video/obsidian/descent-2.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-descent-3-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/descent-3.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/descent-3.mp4' | relative_url }}">Download the video (MP4, 10 MB)</a>
  </video>
  <figcaption><strong>3 of 3.</strong> Down the river at a few hundred metres, across the
  water and onto the valley floor, turning to look back along the river.
  <a href="{{ '/assets/video/obsidian/descent-3.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

## Flights

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-gorge-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/gorge.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/gorge.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption><strong>The gorge.</strong> Upstream between the valley walls toward the snow
  peaks, in low morning sun.
  <a href="{{ '/assets/video/obsidian/gorge.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-river-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/river.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/river.mp4' | relative_url }}">Download the video (MP4, 10 MB)</a>
  </video>
  <figcaption><strong>The river.</strong> Low along the meanders of a lowland river through
  patchy forest, an afternoon of high cloud.
  <a href="{{ '/assets/video/obsidian/river.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-range-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/range.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/range.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption><strong>The range.</strong> At 8 to 10 km over the gorge country into the
  snowy range, the afternoon sun throwing the ridges' shadows across the snow.
  <a href="{{ '/assets/video/obsidian/range.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/obsidian-forest-sunset-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/obsidian/forest-sunset.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/obsidian/forest-sunset.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption><strong>Forest at sunset.</strong> Skimming the treetops at a hundred metres
  into a pink sky.
  <a href="{{ '/assets/video/obsidian/forest-sunset.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>
