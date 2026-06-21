# HyperFrames Agent — Project Guide

This repository is a **HyperFrames** project: an agent-ready workspace for authoring and rendering
**videos as code**. You write plain HTML + CSS + seekable GSAP animations; HyperFrames renders them
into **deterministic MP4/WebM** video. "Write HTML. Render video. Built for agents."

You (Claude Code / Claude cowork) are expected to open this repo and **edit `index.html` (and
sub-compositions under `compositions/`) to cut/produce videos**, then lint, preview, and render.

---

## 📚 READ THE SUMMARY DOCS FIRST — `docs/`

> **If you want to know ANYTHING about HyperFrames — concepts, CLI flags, the Rule of Three,
> data attributes, animation, components, templates, shader transitions, design rules, real use
> cases, troubleshooting — go READ `docs/` BEFORE guessing.** Start at **[`docs/README.md`](docs/README.md)**.
> The docs are the distilled, English source-of-truth summary of the official HyperFrames
> documentation. Do not invent behavior that isn't in the docs.

| When you need to know… | Read |
| --- | --- |
| What HyperFrames is, architecture, pipeline, vs Remotion | [`docs/01-overview.md`](docs/01-overview.md) |
| Install, every CLI command & flag, the production loop | [`docs/02-getting-started.md`](docs/02-getting-started.md) |
| The **Rule of Three**, all `data-*` attributes, composition/clip/track model, variables | [`docs/03-authoring-model.md`](docs/03-authoring-model.md) |
| GSAP, frame adapters, `window.__timelines`, determinism, animation recipes | [`docs/04-animation-and-determinism.md`](docs/04-animation-and-determinism.md) |
| Installing blocks/components from the registry (`add`, `catalog`) | [`docs/05-components-registry.md`](docs/05-components-registry.md) |
| Built-in templates & skeletons, scene durations, aspect ratios | [`docs/06-templates-skeletons.md`](docs/06-templates-skeletons.md) |
| Shader transitions: names, when/how to use, anchor rules | [`docs/07-shader-transitions.md`](docs/07-shader-transitions.md) |
| Design system: typography, palette, grain, `frame.md` | [`docs/08-design-system.md`](docs/08-design-system.md) |
| The agent production workflow + self-review checklist + common mistakes | [`docs/09-agent-workflow.md`](docs/09-agent-workflow.md) |
| Real-world use cases & who uses HyperFrames | [`docs/10-use-cases.md`](docs/10-use-cases.md) |
| Errors, ffmpeg/Chrome setup, lint failures, render≠preview | [`docs/11-troubleshooting.md`](docs/11-troubleshooting.md) |

There is also **live, offline reference** in the terminal:

```bash
npx hyperframes docs <topic>
# topics: data-attributes, gsap, compositions, rendering, examples, troubleshooting
```

---

## Skills — invoke these when present

HyperFrames ships agent **skills** that encode framework-specific patterns. **Prefer them over
generic web knowledge.** Start at **`/hyperframes`** — it routes any "make me a video" intent to the
right workflow (`/product-launch-video`, `/website-to-video`, `/faceless-explainer`,
`/embedded-captions`, `/graphic-overlays`, `/pr-to-video`, `/motion-graphics`, `/general-video`).
Domain skills: `/hyperframes-core`, `/hyperframes-animation`, `/hyperframes-creative`,
`/hyperframes-cli`, `/hyperframes-media`, `/hyperframes-registry`.

> Skills not installed? Run `npx hyperframes skills` (or `npx skills add heygen-com/hyperframes`)
> and restart the agent session. `AGENTS.md` holds the same routing for non-Claude agents.

---

## Commands

```bash
npm run dev          # live preview studio (LONG-RUNNING — run in background, never foreground)
npm run lint         # validate the composition (fast)
npm run check        # lint + validate + inspect (run before considering work done)
npm run render       # render to MP4 (needs ffmpeg + Chrome — see troubleshooting)
npm run doctor       # check system dependencies
npm run publish      # upload and get a shareable link

npx hyperframes add <name>      # install a registry block/component
npx hyperframes catalog         # browse blocks & components
npx hyperframes snapshot        # PNG key-frames for visual verification
npx hyperframes lint --json     # machine-readable, for CI
```

> ⚠️ `npm run dev` is a **long-running server**. In Claude Code, always launch it with
> `run_in_background: true`. Running it in the foreground will time out and kill the preview.

---

## Hard rules (an agent must NOT break)

1. **Rule of Three** — root has `data-composition-id` + `data-width` + `data-height`; every timed
   element has `class="clip"` + `data-start` + `data-duration` + `data-track-index`; GSAP timelines
   are `gsap.timeline({ paused: true })` and registered on `window.__timelines["<composition-id>"]`.
2. **Determinism only** — no `Math.random()`, no `Date.now()`, no `setInterval`/`setTimeout`-driven
   motion, no `repeat: -1`, no network fetches at render time. Drive everything from the timeline.
3. **Video elements** use `muted playsinline`; route audio through a separate `<audio>` clip.
4. **Always `npm run check` after editing.** Fix every error before declaring the task done.
5. **Never use banned/aesthetic-poor fonts** when quality matters — see `docs/08-design-system.md`.
   (The example uses `Inter` because it is auto-resolved by the renderer and lints clean.)

## Production loop

`plan → write HTML → wire seekable GSAP animation → add media → npm run lint/check → npm run dev (preview) → npm run render`

See [`docs/09-agent-workflow.md`](docs/09-agent-workflow.md) for the full loop and self-review checklist.

---

## Output & git policy

- **Rendered video output goes to `out/` and is GITIGNORED. NEVER `git add -f` an MP4/MOV/WebM.**
- Generated artifacts (snapshots, beats, transcripts, captured footage, TTS audio) are also ignored
  — see [`.gitignore`](.gitignore).
- **Source is what we commit**: `index.html`, `compositions/`, config, small input assets, and docs.
- `render` needs `ffmpeg` (+ Chrome headless shell); `preview`/`lint` do not. If `ffmpeg` is
  missing, run `sudo apt install ffmpeg` and `npx hyperframes browser ensure` — see
  [`docs/11-troubleshooting.md`](docs/11-troubleshooting.md).

## Project structure

```
index.html            # main composition (root timeline) — the example title card
compositions/         # sub-compositions (data-composition-src) + installed components/
assets/               # small INPUT media (large media is gitignored)
out/                  # render OUTPUT (gitignored)
docs/                 # ← the English summary docs (read these!)
hyperframes.json      # registry + path config
meta.json             # project metadata
AGENTS.md             # same guidance for non-Claude agents
```
