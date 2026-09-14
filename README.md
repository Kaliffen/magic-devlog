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

Images go in `assets/images/`. The site is served under `/magic-devlog/`, so reference
them through `relative_url` rather than a bare absolute path:

```html
<figure>
  <img src="{{ '/assets/images/whatever.jpg' | relative_url }}" alt="What it shows">
  <figcaption>Caption.</figcaption>
</figure>
```

Two images side by side: `<figure class="figure-grid">` with two `<img>` and one
`<figcaption>`. Keep stills at 1600 px wide, JPEG quality ~88 — they live in git history
forever.

Push to `main` and the Actions workflow builds and deploys automatically.

## Local preview (optional)

Requires Ruby + Bundler:

```
bundle exec jekyll serve
```
