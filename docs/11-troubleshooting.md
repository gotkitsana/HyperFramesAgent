# 11 · Troubleshooting

Run `npx hyperframes doctor` (or `npm run doctor`) first — it checks Node, FFmpeg, FFprobe, Chrome,
Docker, memory, and disk in one shot.

## "FFmpeg not found" / "FFprobe not found"

Local rendering needs FFmpeg (FFprobe ships with it). `preview` and `lint` do **not** need it.

- macOS: `brew install ffmpeg`
- Ubuntu/Debian: `sudo apt install ffmpeg`
- Windows: download from <https://ffmpeg.org/download.html>

> In this repo's environment, ffmpeg may be absent — install it before `npm run render`, or use
> `npx hyperframes cloud` to render without local ffmpeg.

## "Chrome Headless Shell is required"

Local rendering also needs Chrome:

```bash
npx hyperframes browser ensure
```

## "No composition found"

The directory needs an `index.html` with a root `data-composition-id`. This repo already has one;
if you're in a subfolder, `cd` back to the project root.

## Lint errors

Run `npx hyperframes lint` (or `npm run check` for lint + validate + inspect). Common findings:

| Finding | Fix |
| --- | --- |
| Missing `data-composition-id` on root | Add it to the root element (also `data-width`/`data-height`). |
| Missing `class="clip"` on a timed element | Add `class="clip"` so the runtime manages visibility. |
| `font_family_without_font_face` | The font isn't auto-resolved. Add an `@font-face` pointing at a local `.woff2`, or use an auto-resolved font (e.g. `Inter`). |
| `gsap_studio_edit_blocked` (warning) | Informational: elements animated by a registered timeline can't be drag-edited in Studio. Safe to ignore for code-driven comps. |
| Overlapping timelines / invalid `data-*` | Check scene windows tile correctly and attribute values are valid. |
| Text overflow / clipped container / overlapping text (`inspect`) | Reduce text, raise duration, or fix layout; re-run `npm run check`. |

Use `npx hyperframes lint --verbose` for info-level findings and `--json` for CI.

## Preview not updating

The dev server watches the project's `index.html`. Make sure you're editing the right file, and
that `npm run dev` is still running (it's a **long-running** server — in Claude Code, run it with
`run_in_background: true` so it doesn't get killed).

## Render looks different from preview

Local renders can differ due to host fonts and Chrome version. For deterministic output use:

```bash
npx hyperframes render --docker     # pinned Chrome + fonts (needs Docker running)
```

## Scene is invisible for its whole window

Almost always a visibility/shader issue (see [07 · Shader Transitions](07-shader-transitions.md)):

- First anchor in a shader group needs `tl.set("#sN", { opacity: 1 }, startTime)`.
- Non-anchor scenes need `autoAlpha` toggles, not bare `visibility`.

## Shader transitions break / canvas is corrupted (esp. Safari/iframe)

Remove any SVG-filter `data:` URL used as `background-image` (grain). Use CSS radial-gradient grain
instead — see [08 · Design System](08-design-system.md).

## Render is slow

- Use `-q draft` while iterating.
- `-w 4` (4 workers) is usually the sweet spot.
- `npx hyperframes benchmark` finds optimal fps/quality/worker settings.
- Add `--gpu` for hardware FFmpeg encoding where available.

## WebGL / GPU issues during capture

Local renders auto-probe WebGL and fall back to software (SwiftShader). Force behavior with
`--browser-gpu` (require hardware) or `--no-browser-gpu` (force software). Docker mode always uses
software.
