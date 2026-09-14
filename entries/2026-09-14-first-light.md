---
title: First Light
date: 2026-09-14
tag: Meta
---

"First light" is what you call the first real image out of a new
instrument — the moment a telescope or a camera stops being a pile of parts
and starts showing you something. Feels like the right name for entry one of
a devlog about a renderer.

## Why this exists

*Magic*'s source is private — it's a learning project, and the code stays
that way for now. But the process of building it is worth writing down: the
terrain bugs, the frame-budget fights, the moments something clicks and the
planet suddenly looks like a planet instead of a textured ball. A devlog is
the place for that, without needing to open the source up.

## Why it's standalone

This site is deliberately its own repository, separate from the project's
code. Two reasons:

- **The source is private; the devlog isn't.** Keeping them apart means
  writing about the work never risks leaking something that should stay
  closed.
- **Different lifetimes.** A devlog accumulates screenshots for years and
  should be free to grow, get redesigned, or have old entries edited without
  touching engineering history in the actual repo.

Nothing on this site is generated from the source docs. Every entry here is
written for this page, by hand.

## How it's built

Plain Jekyll, deployed to GitHub Pages by a GitHub Actions workflow — push
to `main` and it's live in under a minute. No theme dependency, no build
step beyond what Jekyll does out of the box. Simple on purpose, since the
point is writing, not maintaining a static site generator.

Screenshots and renders will be the bulk of what shows up here over time —
this is a renderer devlog, after all. Expect a lot of images of the same
planet, looking slightly less wrong each time.
