---
layout: default
title: Devlog
permalink: /devlog/
---

# Devlog

Every entry here is written by hand for this page — nothing is pulled
automatically from the project's (private) source repo.

<ul class="entry-list">
{% assign entries = site.entries | sort: "date" | reverse %}
{% for entry in entries %}
  <li class="entry-card">
    <a href="{{ entry.url | relative_url }}">{{ entry.title }}</a>
    {% if entry.date %}<time datetime="{{ entry.date | date_to_xmlschema }}">{{ entry.date | date: "%B %-d, %Y" }}</time>{% endif %}
    {% if entry.excerpt %}<p class="excerpt">{{ entry.excerpt | strip_html | truncatewords: 28 }}</p>{% endif %}
  </li>
{% endfor %}
</ul>
