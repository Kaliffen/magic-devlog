---
layout: default
title: Home
---

# Magic Devlog

Development log for *Magic*. This log is standalone — the game's source is
private, so entries here stand on their own rather than linking back to it.

<ul class="entry-list">
{% assign entries = site.entries | sort: "date" | reverse %}
{% for entry in entries %}
  <li>
    <a href="{{ entry.url | relative_url }}">{{ entry.title }}</a>
    {% if entry.date %}<time datetime="{{ entry.date | date_to_xmlschema }}">{{ entry.date | date: "%B %-d, %Y" }}</time>{% endif %}
  </li>
{% endfor %}
</ul>
