# 06 · Templates & Skeletons

## Built-in `init` templates

Scaffold a project from a template with `npx hyperframes init --example <name>`:

| Template | What it gives you |
| --- | --- |
| `blank` | Empty 1920×1080 composition with a GSAP timeline wired up. Start from scratch. |
| `title-card` | Animated title + subtitle with GSAP fade-in/out. Good for intro cards. |
| `video-edit` | A `<video>` element with trimming, audio, and track controls. Starting point for cutting footage. |

Any directory with an `index.html` can serve as a custom template — copy it manually or build your
own init workflow.

## Skeleton picker (for longer, multi-scene videos)

When designing a full video, pick a **skeleton** sized to the format. Each skeleton is pre-valid
(passes `npx hyperframes lint`) and pre-wires scene structure; you fill in palette, content, and
animation.

| Skeleton | Video type | Duration | Scenes | Aspect |
| --- | --- | --- | --- | --- |
| A | Social reel | 10–15s | 5–7 | 9:16 |
| B | Launch teaser | 15–25s | 7–10 | 16:9 |
| C | Product explainer | 30–60s | 10–18 | 16:9 |
| D | Cinematic title | 45–90s | 7–12 | 16:9 |

## Scene-duration guidance

Size each scene to the amount of text on screen (reading time):

| On-screen text | Min duration |
| --- | --- |
| None (hero, icon) | 1.5–2s |
| 1–3 words (kicker, number) | 2–3s |
| 4–10 words (headline + subhead) | 3–4s |
| 11–20 words (a sentence or two lines) | 4–6s |
| 21–35 words (a paragraph) | 6–8s |
| 35+ words | Split into multiple scenes |

Rules of thumb:
- **Soft ceiling ~5s per scene** unless there's a specific reason (a long paragraph, a slow reveal).
- When you change a scene's duration, **recalculate `data-start` for every later scene** so they
  stay tiled end-to-end (no gaps or accidental overlaps).
- Match scene **count** and **durations** to the video type above.

## Aspect ratios

Set the canvas via the root's `data-width` / `data-height` (and the matching `<meta viewport>`):

| Format | Dimensions |
| --- | --- |
| Landscape 16:9 | 1920×1080 |
| Vertical 9:16 (reels/shorts) | 1080×1920 |
| Square 1:1 | 1080×1080 |

See [08 · Design System](08-design-system.md) for typography/palette and
[09 · Agent Workflow](09-agent-workflow.md) for how to fill a skeleton end-to-end.
