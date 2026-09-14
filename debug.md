---
layout: default
title: Debug
permalink: /debug/
---

- time: {{ site.time }}
- collections: {% for c in site.collections %}{{ c.label }}({{ c.docs | size }}) {% endfor %}
- site.entries size: {{ site.entries | size }}
- plain loop: {% for e in site.entries %}[{{ e.title }} {{ e.date }} {{ e.url }}] {% endfor %}
- sorted: {% assign s = site.entries | sort: "date" %}{% for e in s %}[{{ e.title }}] {% endfor %}
- reversed: {% assign r = s | reverse %}{% for e in r %}[{{ e.title }}] {% endfor %}
