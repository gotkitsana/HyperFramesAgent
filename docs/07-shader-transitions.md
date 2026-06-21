# 07 · Shader Transitions (HyperShader)

HyperFrames includes WebGL transition effects (`@hyperframes/shader-transitions`, aka HyperShader)
that composite two scenes as textures and blend between them with a GPU shader.

## Use shaders sparingly

**~95% of professional video uses hard cuts.** Reserve shaders for **2–3 key moments** — a hero
reveal, an energy shift, the CTA landing. A shader on every cut is the video equivalent of bolding
every word in a paragraph.

- **Minimum transition duration: 0.3s.** Sweet spot: **0.5s.**
- The transition **boundary must fall inside the clip window**:
  `time < boundary < time + duration`.
- Match the shader's energy to the moment: calm → `cross-warp-morph` / `light-leak`;
  high-energy → `glitch` / `chromatic-split`.

## Available shader names

```
domain-warp      ridged-burn        whip-pan          sdf-iris
ripple-waves     gravitational-lens cinematic-zoom    chromatic-split
swirl-vortex     thermal-distortion flash-through-white  cross-warp-morph
light-leak       glitch
```

(Confirm the current set with `npx hyperframes catalog`.)

## Anchor / non-anchor scene visibility (critical)

When a shader transition runs, HyperShader blanks all scenes to `opacity:0` and draws the two
participating scenes itself. This creates two classes of scene:

- **Anchor scenes** (the ones bracketing a shader transition) use `style="opacity:0;"` and let
  HyperShader manage their opacity. **The first anchor in each shader group must also get
  `tl.set("#sN", { opacity: 1 }, startTime)`** — browser mode does not auto-show it, so without
  this it stays invisible for its whole window.
- **Non-anchor scenes** use `style="visibility:hidden;"` **plus explicit `autoAlpha` toggles** in
  the timeline:
  ```js
  tl.set("#sN", { autoAlpha: 1 }, startTime);
  tl.set("#sN", { autoAlpha: 0 }, endTime);
  ```
  Use `autoAlpha` (not bare `visibility`) so that when HyperShader sets `opacity:0`, the override
  covers both properties — otherwise a `visibility:visible` element still renders transparent.

## Do NOT add exit tweens before a shader transition

The shader **is** the exit. Don't fade or slide content off before it — HyperShader needs both
scenes' content fully visible to composite them as textures. (Exit tweens are only for the final
scene, where there's no following shader.)

## Scene skeleton (shader-ready)

```html
<div class="scene clip" id="s3" data-start="X" data-duration="Y" style="visibility:hidden;">
  <div class="grain"></div>
  <div class="scene-content">
    <!-- content -->
  </div>
</div>
```

Anchor scenes swap `visibility:hidden;` for `opacity:0;`.

## Gotcha: grain / backgrounds

Do **not** use SVG-filter `data:` URLs as `background-image` for grain — they corrupt the canvas in
Safari and break all shader transitions inside iframes. Use a **CSS radial-gradient grain** instead
(see [08 · Design System](08-design-system.md)).
