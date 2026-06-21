# 08 · Design System

Good HyperFrames video is good design first. This summarizes the design rules from the official
HyperFrames design guide.

## `frame.md` / DESIGN.md — design tokens for video

Every brand has a `design.md` (web design system), but none target the **camera's** constraints.
HyperFrames introduces a superset spec — sometimes called **`frame.md`** / `DESIGN.md` — that keeps
atomic design tokens (palette, type scale, spacing) **and** adds video-specific scale so agents can
compose videos without guessing at sizing or fighting web chrome. Define your palette and type as
`:root` CSS custom properties and reuse them across scenes:

```css
:root {
  --bg: #0a0a0f;
  --fg: #f5f5f7;
  --accent: #7aa2ff;
  --muted: #8b8b9a;
  --display: 200px;   /* hero */
  --headline: 96px;
  --body: 24px;
  --label: 16px;
}
```

## Typography

- **Weight contrast must be dramatic** — pair `300` vs `900`, not `400` vs `700`.
- **Minimum sizes for legibility on screen:** headlines **60px+**, body **20px+**, labels **16px+**.
- Use `font-variant-numeric: tabular-nums` on number columns/counters so digits don't jitter.
- **Avoid over-used "default" fonts** when quality matters. The design guide discourages reaching
  for: Inter, Roboto, Open Sans, Poppins, Outfit, Playfair, Fraunces, EB Garamond, Nunito, Source
  Sans, PT Sans, Syne, Cinzel, Prata, Bodoni Moda, Arimo, Lato, Noto Sans. Pick real typefaces with
  strong character and dramatic weight contrast.

> ⚠️ **Lint vs aesthetics:** the linter only errors on a font that has **no `@font-face` and isn't
> in the renderer's auto-resolved list** (`font_family_without_font_face`). It does **not** enforce
> the aesthetic "avoid" list. The repo's example uses `Inter` precisely because it auto-resolves and
> lints clean — for production work, supply a custom font via `@font-face` pointing at a local
> `.woff2` (e.g. under `capture/assets/fonts/`) and reference it.

## Palette

- Match the brief's colors **exactly** (hex codes from the brand).
- Keep a small, deliberate palette: background, foreground, one or two accents, a muted tone.

## Grain & texture (do it the safe way)

Use a **CSS radial-gradient grain** — never an SVG-filter `data:` URL (those corrupt the canvas in
Safari and break shader transitions in iframes — see [07](07-shader-transitions.md)).

```css
.grain {
  position: absolute; inset: 0; pointer-events: none;
  background-image:
    radial-gradient(rgba(255,255,255,0.08) 1px, transparent 1.2px),
    radial-gradient(rgba(0,0,0,0.18) 1px, transparent 1.2px);
  background-size: 3px 3px, 5px 5px;
}
```

## Motion is part of the design

A still title on a still background reads as a slide. Give every scene an entrance tween and (for
scenes >4s) at least one mid-scene activity. See
[04 · Animation & Determinism](04-animation-and-determinism.md) for recipes and the per-scene
baseline.
