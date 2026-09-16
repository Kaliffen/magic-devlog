---
layout: default
title: Intro
permalink: /
---

<div class="hero">
  <h1>Magic — a planet-scale renderer, built and logged in the open</h1>
  <p class="lede">
    Magic is a Vulkan renderer that flies from orbit down to the ground on
    measured Earth terrain — one camera, one world, no seam between the disc
    of the planet and standing on a ridge at 1.7 m. This devlog tracks the
    build: what changed, what broke, and why.
  </p>
  <div class="hero-actions">
    <a class="btn btn-primary" href="{{ '/devlog/' | relative_url }}">Read the devlog</a>
    <a class="btn" href="{{ '/project/' | relative_url }}">What is Magic?</a>
  </div>
</div>

<hr>

<h2>Six places, one renderer</h2>
<p>Headless stills from the measured Earth world, unedited. Every one of these is the same
camera and material pipeline that draws the ground under your feet at 1.7 m.</p>
<div class="site-showcase">
  <a class="site-card" href="{{ '/assets/images/site-patagonia-ice.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-patagonia-ice.jpg' | relative_url }}" alt="The Southern Patagonian Ice Field from orbit, braided glacial rivers below">
    <span class="site-caption">Southern Patagonian Ice Field, from orbit</span>
  </a>
  <a class="site-card" href="{{ '/assets/images/site-kilimanjaro.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-kilimanjaro.jpg' | relative_url }}" alt="Kilimanjaro's Kibo summit at the equatorial snow line, noon">
    <span class="site-caption">Kilimanjaro, the Kibo summit at noon</span>
  </a>
  <a class="site-card" href="{{ '/assets/images/site-bosphorus.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-bosphorus.jpg' | relative_url }}" alt="The Bosphorus strait mouth, forested shoreline and beaches">
    <span class="site-caption">The Bosphorus mouth, looking south along the strait</span>
  </a>
  <a class="site-card" href="{{ '/assets/images/site-vatnajokull.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-vatnajokull.jpg' | relative_url }}" alt="Vatnajokull ice cap from 100 km, broken cloud over the glacier">
    <span class="site-caption">Vatnaj&ouml;kull ice cap, 100 km up</span>
  </a>
  <a class="site-card" href="{{ '/assets/images/site-monte-rosa.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-monte-rosa.jpg' | relative_url }}" alt="Monte Rosa across the Alps from Turin at sunrise">
    <span class="site-caption">Monte Rosa from Turin, sunrise</span>
  </a>
  <a class="site-card" href="{{ '/assets/images/site-maunakea.jpg' | relative_url }}">
    <img src="{{ '/assets/images/site-maunakea.jpg' | relative_url }}" alt="A ridge on Mauna Kea's flank at dusk">
    <span class="site-caption">Mauna Kea, dusk on the summit road</span>
  </a>
</div>

<h2>Seventeen hours in thirty-eight seconds</h2>
<p>A time-lapse recorded in the engine from one viewpoint 686 m over the Hardangerfjord and
exported headless through the same renderer, night to night at twice speed. The terrain
and the clouds shadow the air, the sky sets the exposure once the ground goes dark, and the
twilight arch fades the way an eye sees it rather than the way a long exposure records it.</p>
<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/recorded-flight-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/recorded-flight.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/recorded-flight.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption>Hardangerfjord, 59.9&deg; N: a full day from the same spot, 1280&times;720 at 30 fps.
  <a href="{{ '/assets/video/recorded-flight.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<h2>From orbit to a walk on Himmelbjerget</h2>
<p>An authored spaceplane approach, rendered headless: a spiral from 6,000 km that breaks
off at re-entry height over the Bay of Biscay, a banked glide through waypoints over the
Paris basin, the Ardennes, the north German plain and the Elbe, decelerating up Jutland to a
hover over the summit, and a vertical touchdown into the walking view at 1.7 m. One camera,
one world, from the disc of the planet to standing in the trees.</p>
<figure>
  <video class="flight-video" controls playsinline preload="metadata"
    poster="{{ '/assets/images/himmelbjerget-flight-poster.jpg' | relative_url }}">
    <source src="{{ '/assets/video/himmelbjerget-flight.mp4' | relative_url }}" type="video/mp4">
    <a href="{{ '/assets/video/himmelbjerget-flight.mp4' | relative_url }}">Download the video (MP4, 9 MB)</a>
  </video>
  <figcaption>Himmelbjerget, 56.1&deg; N: 115 seconds of flight at 2.2&times; speed, 1280&times;720 at 30 fps.
  <a href="{{ '/assets/video/himmelbjerget-flight.mp4' | relative_url }}">Open the MP4</a>.</figcaption>
</figure>

<h2>Latest entries</h2>
<ul class="entry-list">
{% assign entries = site.entries | sort: "date" | reverse %}
{% for entry in entries limit: 4 %}
  <li class="entry-card">
    <a href="{{ entry.url | relative_url }}">{{ entry.title }}</a>
    {% if entry.date %}<time datetime="{{ entry.date | date_to_xmlschema }}">{{ entry.date | date: "%B %-d, %Y" }}</time>{% endif %}
    {% if entry.excerpt %}<p class="excerpt">{{ entry.excerpt | strip_html | truncatewords: 28 }}</p>{% endif %}
  </li>
{% endfor %}
</ul>
<p><a href="{{ '/devlog/' | relative_url }}">All entries →</a></p>
