# 09 · Agent Workflow

How AI agents should produce a HyperFrames video, end to end.

## Division of labor: Design → Code

HyperFrames frames agent work as two passes (think *first-cut editing*, not final production):

- **Claude Design** creates the **first cut** — brand identity, layout, scene content, and initial
  GSAP animations as plain HTML + GSAP. Deliverable: a **structurally valid** project (passes
  `npx hyperframes lint`) with full motion from the draft.
- **Claude Code** then **refines the feel** by watching playback: adjusts ease curves
  (`power3.out` → `expo.out`), tightens stagger timing, trims overlong scenes, adds richer
  mid-scene activity where things feel static, swaps shaders if an energy shift needs a different
  effect, and verifies cross-browser/snapshot integrity.

Your job as the authoring agent: **structural validity + brand identity + full motion from the
draft.** Polishing the timing comes after, against real playback.

## The production loop

```
1. Plan       — pick a brief (with at least one visual reference: hex codes, a named aesthetic,
                or a known brand), choose a skeleton/format (docs/06), set the canvas size.
2. Write HTML — build scenes with the Rule of Three (docs/03). Fill :root tokens, then populate
                each scene's content one by one.
3. Animate    — wire a paused GSAP timeline on window.__timelines; entrance + mid-scene motion,
                varied eases, deterministic only (docs/04).
4. Media      — add video (muted playsinline) / audio clips; install registry blocks if useful
                (docs/05). Shaders at 2–3 key moments only (docs/07).
5. Lint       — npm run check (lint + validate + inspect). Fix EVERY error.
6. Preview    — npm run dev (run in background) and watch it play start to finish.
7. Render     — npm run render → out/ (needs ffmpeg + Chrome).
```

## Self-review checklist

### Structural (must pass — hard to fix later)
- [ ] Every timed element has `class="clip"` + `data-start` + `data-duration` + `data-track-index`.
- [ ] Root has `data-composition-id` + `data-width` + `data-height`.
- [ ] `window.__timelines["<id>"]` key matches the root's `data-composition-id`.
- [ ] Timeline is `{ paused: true }`.
- [ ] Scene windows tile end-to-end — no unintended gaps or overlaps.
- [ ] No exit tweens except on the final scene; **no exit tweens before a shader transition**.
- [ ] Shader transitions: boundary inside the window; nothing shorter than 0.3s.
- [ ] Anchor scenes use `opacity:0;` + first-anchor `tl.set({opacity:1})`; non-anchors use
      `visibility:hidden;` + `autoAlpha` toggles (docs/07).
- [ ] No `Date.now()`, `Math.random()`, `setInterval` motion, `repeat:-1`, or render-time fetches.
- [ ] No SVG-filter `data:` grain.
- [ ] `npm run check` → **0 errors**.

### Brand + content
- [ ] Colors match the brief exactly; no banned/default-feeling fonts when quality matters.
- [ ] Font sizes ≥ 60px headlines / 20px body / 16px labels.
- [ ] Every scene has meaningful content (no placeholder text).
- [ ] Scene count and durations match the chosen video type.

### Animation baseline
- [ ] Every scene has ≥1 entrance tween.
- [ ] Scenes >4s have ≥1 mid-scene activity; no completely static scenes.
- [ ] Text is readable in the time it's on screen.

## Top common mistakes

1. **Missing first-anchor opacity toggle** → scene invisible all window. Add
   `tl.set("#sN", { opacity: 1 }, startTime)`.
2. **Non-anchor uses `visibility` instead of `autoAlpha`** → goes transparent when HyperShader
   blanks scenes.
3. **Exit tweens before a shader transition** → the shader IS the exit; keep content visible.
4. **SVG-filter grain in `background-image`** → Safari/iframe canvas corruption; use CSS gradient.
5. **Static scenes** → add mid-scene motion (counter, float, glow, zoom).
6. **Shader on every cut** → reserve for 2–3 moments; hard-cut the rest.
7. **Default-feeling fonts / weak weight contrast** → pick real typefaces, `300` vs `900`.

## Operational note for Claude Code

`npm run dev` is a **long-running** server — launch it with `run_in_background: true`. Use
`npx hyperframes snapshot` to grab PNG key-frames for visual verification without a full render.
