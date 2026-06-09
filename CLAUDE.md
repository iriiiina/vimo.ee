# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static HTML/CSS landing page for [vimo.ee](https://vimo.ee), served by GitHub Pages (`CNAME` → `vimo.ee`). No build step, no JS framework, no package manager — edits to HTML/CSS in `master` go live on push.

## Structure

- `index.html` — the landing page (portfolio + people + footer). Uses `css/style.css`.
- `privacy.html`, `riigipyhad/index.html` — standalone pages with **inline CSS**; they do **not** share `css/style.css`. Style changes to the main site must be replicated here if visual consistency matters.
- `img/` — every photo ships in both `.png` (or `.jpeg`) and `.webp`, served via `<picture>` with `<source type="image/webp">` first. When adding a portfolio image, generate both formats (Squoosh) and add both `<source>` tags.
- `sitemap.xml` — update `<lastmod>` for `https://vimo.ee/` when meaningfully editing `index.html`.

## SEO and schema are load-bearing

The HTML is hand-tuned for SEO and schema.org structured data — these are the primary reason for most recent commits. When editing `index.html`:

- Preserve `itemscope` / `itemtype` / `itemprop` attributes. The body is a `WebPage`, `#portfolio` is a `Collection` of `WebPage`s, `#people` contains `Person`s, the footer is a `LocalBusiness` with a nested `PostalAddress`.
- Validate changes at <https://validator.schema.org/> (schema) and <https://konversioon.ee/> (SEO) before considering a change done — this is the established workflow per README.
- Keep meta tags (`description`, `og:*`, `canonical`) in sync when titles or descriptions change.

## Workflow

- No commands to run. Preview locally by opening `index.html` in a browser, or `python3 -m http.server` from the repo root if relative paths matter.
- Deploy = `git push` to `master`. GitHub Pages serves directly from the root.
