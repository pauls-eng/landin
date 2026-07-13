# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A single-page, static HTML architectural competition board ("lámina de concurso") for the Quálitas San Jerónimo 492 project by RE+ARQ. There is no build system, package manager, linter, or test suite — the entire deliverable is `index.html` (all CSS inline in a `<style>` block) plus four PNG renders in `images/`.

The output is a print-ready A1 portrait poster (594 × 841 mm), viewed in a browser and exported via Print → "Save as PDF" at A1 size with no margins.

## Development workflow

- **Run:** open `index.html` directly in a browser (or `python3 -m http.server` and browse to it). No install step.
- **Verify changes:** check both the on-screen view and the print preview (`@page` is A1 portrait, zero margin). The board must remain a single page — `break-inside:avoid` is set on `.board`.
- Content is in Spanish (`lang="es"`); keep copy, captions, and comments in Spanish.

## Architecture of index.html

- **Proportion locking:** `.board` uses `aspect-ratio: 594/841` and `container-type: size`, and every dimension inside it is expressed in container query units (`cqw`/`cqh`) so the whole layout scales uniformly on screen and in print. When adding or adjusting elements, use `cqw`/`cqh` — never `px`, `rem`, or viewport units.
- **Layout:** `.board` is a 6-row CSS grid with percentage row heights: header (8%), thesis band (7%), hero image (42%), technical row of 3 columns (26%), concept band of 4 columns (11%), footer (6%). Row percentages must sum to 100.
- **Image slots:** each `.slot` pairs a dashed placeholder (`.slot__ph`) with an `<img>`. Inline `onload` hides the placeholder when the image loads; `onerror` hides the broken image so the placeholder shows through. Images use `object-fit: contain` (uncropped, centered on cream — switch to `cover` only if cropping is desired).
- **Design tokens:** brand colors and fonts are CSS custom properties on `:root` (`--cream`, `--ink`, `--gray`, `--yellow` accent, `--line`; `Fraunces` serif / `Archivo` sans from Google Fonts). Use the tokens rather than hard-coded values.
- **Icons:** the concept band uses inline SVG line icons styled globally (`stroke: var(--ink)`, `fill: none`).

## Image conventions

The four PNGs in `images/` must keep these exact names (referenced from `index.html`; see `images/README.md`):

| File | Aspect | Content |
|---|---|---|
| `01-hero.png` | 4:5 | Exterior render, Av. San Jerónimo access |
| `02-seccion.png` | 3:4 | Longitudinal section, 1:200 |
| `03-atrio.png` | 3:4 | Central atrium from lobby |
| `04-axonometrico.png` | 3:4 | Exploded axonometric |

If an image is missing, its slot shows the dashed placeholder with the expected ratio — no HTML changes needed to swap images.
