# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run dev` — serve slides locally via `reveal-md content` (live reload).
- `npm run build` — rebuild the static site into `docs/` (wipes `docs/` first). `docs/` is the GitHub Pages publish target, so it is checked in.

To preview a single deck: `npx reveal-md content/<name>.md` or `npx reveal-md content/<dir>`.

## Architecture

This is a collection of conference/standards talks rendered with [reveal-md](https://github.com/webpro/reveal-md) (Markdown → Reveal.js).

- `content/` — source of truth. Each talk is either a single `.md` file or a directory containing `slides.md` plus its own assets. Slides use reveal-md's `---` (horizontal) and `--` (vertical) separators and front-matter for per-deck overrides.
- `assets/` — shared chrome applied to every deck:
  - `footer-template.html` — the `template` referenced in `reveal-md.json`; wraps each deck. Renders an `#footer` div with `@ryzokuken` for decks that don't define their own footer.
  - `listing-template.html` — the `listingTemplate` referenced in `reveal-md.json`. Customises reveal-md's deck-index page (`docs/index.html`) so it matches ryzokuken.dev's nav + card aesthetic (JetBrains Mono via Google Fonts, TC39 Orange accent, hard-edged offset shadows, sticky nav, theme switcher with `data-theme` + `localStorage`). Self-contained: tokens are inlined; no external CSS dependency beyond the Google Fonts link.
  - `style.css` — global CSS injected into every deck (currently empty, available for cross-deck tweaks).
  - `theme/igalia.css`, `theme/images/`, `theme/fonts/` — Igalia-branded reveal.js theme + its assets, opt-in per deck.
  - `theme/ryzokuken.css` — personal `ryzokuken.dev` reveal.js theme (JetBrains Mono, TC39 Orange `#FC7C00`, brutalist/terminal). Opt-in per deck — **not** the global default. Designed for the recurring ECMA-402 status-update shape; not appropriate for every talk. Hides `#footer` since it renders its own via `::after`.
  - `theme/fonts/JetBrainsMono-Variable.woff2` — variable font referenced by `ryzokuken.css` at `fonts/…` relative to the theme CSS.
  - `template.html` — additional shared template bits.
- `reveal-md.json` — reveal-md CLI config. Global default: `theme: "blood"` + the footer template + `style.css`. Decks opt into a different theme via front-matter (`<!-- .slide: -->` blocks or reveal-md's `--theme` flag), e.g. point at `./assets/theme/ryzokuken.css` or `./assets/theme/igalia.css`.
- `package.json` — `build` copies `assets/theme/fonts/` into the static output (so decks that opt into `ryzokuken.css` find the font).
- `reveal.json` — Reveal.js runtime config (slide numbers, height, transition, markdown options). Applies to all decks.
- `docs/` — built static output, served by GitHub Pages. **Do not hand-edit**; regenerate with `npm run build`.

When adding a new talk, drop a `.md` (or directory with `slides.md` + assets) under `content/`. Per-deck front-matter can override the global theme/template/CSS from `reveal-md.json`.

### `ryzokuken.css` slide classes

Tag a slide with reveal-md's per-slide attribute (`<!-- .slide: class="…" -->`) to opt into the theme's recurring layouts:

- `title` — first slide. `> ` prompt heading, byline pill (h3:first-of-type), meeting-meta h3s.
- `divider` — section divider. Add `<div class="num">02</div>` for the faint ghost number in the corner; an optional `.lbl` paragraph above the heading reads as `// part one`.
- `consensus` — the recurring "Consensus? 😇" question slide.
- `thanks` — closing slide. Pair with `<div class="handle">ryzokuken</div>` for the `@`‑prefixed handle line.
- `tight` — modifier for dense bullet + code slides at 1280×720.
- (no class) — default: `$ ` h2 prompt, orange-dash bullets, code block with offset orange shadow.

Theme flip: `<body data-theme="light">` (or `class="light"` / `"dark"` on a single `<section>`). Add `data-no-footer` to a section to suppress the `@ryzokuken` footer (e.g. on the title slide). The font is loaded relative to the CSS, so the theme must stay in `assets/` alongside `assets/fonts/`.
