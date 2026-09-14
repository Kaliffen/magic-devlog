---
title: "Atmosphere Revamp: The Paper, and Bennett's Fit for Aerosols"
date: 2026-09-14 20:00:00 +0200
slug: atmosphere-revamp
tag: Atmosphere
description: Reimplemented the atmosphere to follow the reference paper more closely and plugged in Bennett's fit for aerosol phase. A gallery from ocean to orbit.
---

The atmosphere has been reimplemented to follow
[Schneegans 2024](https://doi.org/10.1145/3641519.3657424) more closely, and the aerosol
phase now runs on Bennett's fast fit by default instead of only the numerical Mie tables.
Below is the showcase gallery, unedited, from the ocean to orbit.

<div class="figure-grid">
  <figure>
    <img src="{{ '/assets/images/ar-daylight.jpg' | relative_url }}" alt="Daylight over the ocean">
    <figcaption><strong>Daylight over the ocean.</strong> Low-altitude haze and the daylight aureole. 0.1 km, EV 15, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-blue-hour.jpg' | relative_url }}" alt="Blue hour">
    <figcaption><strong>Blue hour.</strong> Sun 4 degrees below the horizon. 0.1 km, EV 10, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-sunrise.jpg' | relative_url }}" alt="Sunrise">
    <figcaption><strong>Sunrise.</strong> A clear view along the horizon. 0.1 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-aureole.jpg' | relative_url }}" alt="Aureole close-up">
    <figcaption><strong>Aureole close-up.</strong> 14-degree field of view; direct solar disc disabled. 0.1 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-dry-air.jpg' | relative_url }}" alt="Dry air">
    <figcaption><strong>Dry air.</strong> 20% relative humidity; matched camera and exposure with the next view. 0.1 km, EV 12, RH 20%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-humid-air.jpg' | relative_url }}" alt="Humid air">
    <figcaption><strong>Humid air.</strong> 90% relative humidity; matched camera and exposure with the previous view. 0.1 km, EV 12, RH 90%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-cloud-sunset.jpg' | relative_url }}" alt="Cloud sunset">
    <figcaption><strong>Cloud sunset.</strong> Warm cloud illumination above the horizon. 1.5 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-above-clouds.jpg' | relative_url }}" alt="Above the lower clouds">
    <figcaption><strong>Above the lower clouds.</strong> Clouds and atmospheric light seen from above. 7 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-ascent.jpg' | relative_url }}" alt="Ascent">
    <figcaption><strong>Ascent.</strong> The horizon at 10 km. 10 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-stratosphere.jpg' | relative_url }}" alt="Stratosphere">
    <figcaption><strong>Stratosphere.</strong> The thinning atmosphere at 30 km. 30 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-edge-of-space.jpg' | relative_url }}" alt="Edge of space">
    <figcaption><strong>Edge of space.</strong> The curved limb at 100 km. 100 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-orbital-limb.jpg' | relative_url }}" alt="Orbital limb">
    <figcaption><strong>Orbital limb.</strong> The illuminated limb and stars at 400 km. 400 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-limb-close.jpg' | relative_url }}" alt="Limb close-up">
    <figcaption><strong>Limb close-up.</strong> An 18-degree view of the atmospheric layers. 400 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-stars-and-limb.jpg' | relative_url }}" alt="Stars above the night limb">
    <figcaption><strong>Stars above the night limb.</strong> A darker limb with the sun farther below the horizon. 400 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-open-space.jpg' | relative_url }}" alt="Open space">
    <figcaption><strong>Open space.</strong> The mostly white star field and Milky Way. 400 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-mountain-sunrise.jpg' | relative_url }}" alt="Mountain sunrise">
    <figcaption><strong>Mountain sunrise.</strong> Measured terrain near Everest, viewed from 10 km. 10 km, EV 12, RH 70%.</figcaption>
  </figure>
  <figure>
    <img src="{{ '/assets/images/ar-coastal-daylight.jpg' | relative_url }}" alt="Coastal daylight">
    <figcaption><strong>Coastal daylight.</strong> Measured Table Bay terrain looking toward Table Mountain. 0.1 km, EV 15, RH 70%.</figcaption>
  </figure>
</div>

Bloom and the direct solar disc are disabled throughout: the bright aureole comes from
atmospheric scattering alone. Exposure is fixed per view, the dry/humid pair shares camera
and exposure, and relative humidity is manually held for reproducibility. Most of these are
the stub terrain oracle; the mountain and coastal views use measured DEM terrain. These are
appearance captures, not performance or terrain-streaming acceptance tests.
