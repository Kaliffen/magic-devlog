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
