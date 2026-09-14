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
