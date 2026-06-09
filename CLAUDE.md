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

- Preserve `itemscope` / `itemtype` / `itemprop` attributes. The body is a `WebPage`, `#portfolio` is a `Collection` of `WebPage`s, `#people` contains `Person`s, the footer is a `LocalBusiness` with a nested `PostalAddress`. A JSON-LD `@graph` in `<head>` mirrors the `LocalBusiness` + `WebSite` for richer local-search results — it complements, it does not replace, the microdata.
- Validate changes at <https://validator.schema.org/> (schema) and <https://konversioon.ee/> (SEO) before considering a change done — this is the established workflow per README.
- Keep meta tags (`description`, `og:*`, `twitter:*`, `canonical`) **and** the JSON-LD block in sync when titles, descriptions, or business details change.

## Accessibility, SEO, and performance are first-class concerns

Every HTML/CSS edit can affect all three. When asked "any improvements?" — or even when not asked but the change touches the surrounding area — scan for concrete issues across all of these, not just the dimension the user named. Patterns this codebase already uses, and should keep using:

- **Accessibility:** mark decorative content with `aria-hidden="true"` (the `↓` arrow, icon SVGs whose parent `<a>` already has `aria-label`); give every landmark `<section>` an accessible name via `aria-labelledby="<heading-id>"`; keep exactly one `<h1>`; every `:hover` color rule should have a matching `:focus-visible`.
- **SEO:** meta description ~150–160 chars, with location and concrete services; use **absolute URLs** in `og:*` / `twitter:*` / JSON-LD (relative paths break on social scrapers); keep `canonical`, `og:url`, sitemap entries, and internal links consistent on trailing slashes; include `og:image:alt` and a `summary_large_image` Twitter Card.
- **Performance:** above-the-fold images (header logo, first portfolio item) must **not** have `loading="lazy"` — it delays LCP. Every `<img>` keeps explicit `width`/`height` to prevent CLS. Every photo ships `.webp` + `.png`/`.jpeg` fallback via `<picture>`.

## Workflow

- No commands to run. Preview locally by opening `index.html` in a browser, or `python3 -m http.server` from the repo root if relative paths matter.
- Deploy = `git push` to `master`. GitHub Pages serves directly from the root.
