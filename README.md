# magic-devlog

Standalone development log for *Magic*. The game's source lives in a private
repo; this log is deliberately independent of it — entries are hand-written,
not generated from that repo's docs.

Built with Jekyll, deployed to GitHub Pages via GitHub Actions.

## Adding an entry

Create a new file in `entries/`, named `entries/YYYY-MM-DD-slug.md`:

```markdown
---
title: My entry title
date: YYYY-MM-DD
---

Body text, images, etc.
```

Images go in `assets/images/`, referenced as `/assets/images/whatever.png`.

Push to `main` and the Actions workflow builds and deploys automatically.

## Local preview (optional)

Requires Ruby + Bundler:

```
bundle exec jekyll serve
```
