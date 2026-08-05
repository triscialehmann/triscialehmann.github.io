# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Triscia Lehmann's personal portfolio/design site (`TRISH.VISUALS`), hosted on GitHub Pages directly from the `main` branch of this repo (`triscialehmann/triscialehmann.github.io`). There is no build system, package manager, bundler, or test suite — every `.html` file is served as-is. There is no local dev server config either; open the HTML files directly in a browser (or use any static file server) to preview changes.

## Two distinct page families

The site is made of two unrelated codebases living side by side:

1. **Webflow-exported pages** — `home.html`, `about.html`, `work.html`, `blog.html`, `contact.html`, `shop.html`, everything under `post/`, `projects/`, and `utiliy-pages/`. These all share `css/webflow-style.css`, `js/jquery.js`, and `js/webflow-script.js`, and carry Webflow's generated markup/data attributes (`data-wf-page`, `data-w-id`, `w-mod-js`, etc.). **Do not hand-edit the Webflow-generated structural markup, inline `[data-w-id="..."]` transform styles, or class names** — this content is exported from Webflow and re-exporting will overwrite manual changes. Safe edits are text content, links, and images; anything structural should ideally be changed in Webflow and re-exported.
2. **Standalone custom pages** — `index.html`, `portal.html`, and `under-construction.html`. These are hand-written, self-contained HTML/CSS/JS with no external framework and no shared stylesheet with the Webflow pages. Treat these as regular front-end code you can freely edit.

## `index.html` — password gate in front of the site

`index.html` is a self-contained password gate: a single hardcoded `PASSWORD` constant checked client-side in inline `<script>`, redirecting to `DESTINATION` (`home.html`) on success. It also embeds a third-party page-visit counter (hitwebcounter.com `<img>` badge).

This is a soft/cosmetic gate, not real access control — the password lives in plaintext in the page source and is visible to anyone via "View Source" or devtools, and `home.html`/every other page remains directly reachable by URL regardless of the gate. Don't treat it as protecting sensitive content.

## `portal.html` — retro landing-page game (not currently linked from `index.html`)

`portal.html` is a single-file retro/pixel-art canvas game (font: `Press Start 2P` / `VT323`, `<canvas id="game-canvas">`):

- The player walks a sprite around a tile-based world (`TILE`, `WORLD_W`, `WORLD_H` constants) using arrow keys/WASD on desktop or an on-screen D-pad on mobile (`isMobile` branches control input handling).
- Three `portals` (world positions) correspond to `portalData` entries (`ABOUT ME`, `PROJECTS`, `CONTACT`). Walking into one opens `#modal-overlay`/`#modal` with content built inline in JS — there's a hardcoded projects list (name/desc/tags/link) that links out to `projects/*.html` pages or `under-construction.html` as placeholders.
- A `PORTFOLIO` button links out to `home.html`, which is the "real" Webflow-based multi-page portfolio site.
- `#portal-counter` / `.portal-dot` tracks which portals have been visited.

When asked to change portal content, links, or the project list on this page, edit the inline `<script>` in `portal.html` directly (`portalData`, the projects array, and the modal-building functions) — there's no separate JS file or template for this page.

## Content structure of the Webflow pages

- `home.html` is the main portfolio landing page (hero, services, featured work).
- `about.html`, `work.html`, `blog.html`, `contact.html`, `shop.html` are top-level sections.
- `projects/*.html` are individual case-study pages (e.g. `khail.html`, `rush.html`, `shop-nichi.html`, `sintralizedph.html`, `canvaelements.html`, `commissions.html`), linked from the projects list.
- `post/*.html` are individual blog posts, linked from `blog.html`.
- `utiliy-pages/license.html` is a Webflow-generated utility page (typically unused/boilerplate — check before editing).
- `images/` holds all site assets, including Webflow's multi-resolution exports (e.g. `foo-p-500.jpg`, `foo-800.jpg`, `foo-1080.png` are responsive/srcset variants of the same source image — keep the naming convention if adding new responsive images).
- Large PDFs in the repo root (`DAL.pdf`, `SP1.pdf`, `dami.pdf`, `dv.pdf`, `r1.pdf`) are downloadable portfolio/resume assets linked from pages; they are not code.

## Editing conventions

- Since there's no build step, any HTML/CSS/JS edit is immediately "deployed" once pushed to `main` (GitHub Pages serves straight from the repo). There is no staging environment — double-check changes before pushing.
- Analytics: Webflow pages include a Google tag (`gtag.js`, measurement ID `G-ESL9QMQ6LR`) in the `<head>`. Preserve this when editing page heads.
- Keep edits to Webflow-exported files minimal and content-focused; anything requiring structural/layout changes is better done in Webflow itself and re-exported, to avoid drifting from what Webflow would regenerate.
