# 10 · Use Cases

## Who uses HyperFrames

- **HeyGen** — HyperFrames is developed by HeyGen and used in production there.
- **tldraw** and **TanStack** — cited as teams that have adopted it.
- Open-source community via the GitHub repo (`heygen-com/hyperframes`), Discord, and the
  playground at hyperframes.dev.

## What people build with it

HyperFrames is well-suited to **programmatic, templated, and agent-generated** video where
HTML/CSS is a natural authoring surface:

| Use case | Why HyperFrames fits |
| --- | --- |
| **Product launch / feature announcement videos** | Brand-consistent, regenerable from a brief or script; the built-in `/product-launch-video` workflow targets 60–90s promos. |
| **Website → video** | Capture a site (`hyperframes capture`) and turn it into a tour/showcase/social clip. |
| **Faceless explainers** | Turn an article/topic/notes into a 60–90s explainer with kinetic type and captions. |
| **PR / changelog walkthroughs** | Turn a GitHub PR into a 30–90s code-change explainer (diffs, narration, captions). |
| **Captions & subtitles on existing footage** | Add a caption rail/embed to a talking-head MP4 without touching the footage. |
| **Graphic overlays on interviews/podcasts** | Lower-thirds, kinetic titles, data callouts, pull-quotes synced to the transcript. |
| **Motion graphics (<10s)** | Stat count-ups, charts, logo stings, animated tweets/headlines, transparent overlays. |
| **Data visualizations** | Animated charts/counters driven deterministically by the timeline. |
| **Docs-to-video** | Convert documentation into narrated, animated walkthroughs. |
| **Automated content pipelines** | Determinism makes it safe to render videos in CI from data/templates at scale. |

## Why it suits agents specifically

- **Agents already write HTML/CSS/JS** — no new component DSL to learn.
- **Deterministic output** — "same input → same frames" makes automated/regression rendering safe.
- **CLI is non-interactive by default** — explicit flags and `--json` output fit automation.
- **Skills + workflows** route a plain-language request ("make me a 15s intro about X") to the right
  production flow (see `CLAUDE.md` and [02 · Getting Started](02-getting-started.md)).
- **Templated rendering** via composition variables (`--variables`) → one composition, many videos
  (see [03 · Authoring Model](03-authoring-model.md)).

## Distribution

- `npx hyperframes render` → local MP4/WebM in `out/`.
- `npx hyperframes publish` → upload and get a stable shareable URL.
- `npx hyperframes cloud` / `lambda` / `cloudrun` → render at scale without local Chrome/ffmpeg.
