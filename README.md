# HyperFrames Agent

An agent-ready [**HyperFrames**](https://hyperframes.heygen.com/) project for authoring and
rendering **videos as code**. Write plain HTML + CSS + seekable GSAP animations; HyperFrames renders
them to **deterministic MP4/WebM**. Built so Claude Code / Claude cowork can open the repo and cut
videos directly.

## Quickstart

```bash
npm run dev      # live preview studio (run in background)
npm run lint     # validate the composition
npm run check    # lint + validate + inspect
npm run render   # render to MP4 → out/  (needs ffmpeg + Chrome)
npm run doctor   # check system dependencies
```

> **Requirements:** Node.js 22+. Rendering also needs **FFmpeg** (`sudo apt install ffmpeg` /
> `brew install ffmpeg`) and Chrome (`npx hyperframes browser ensure`). Preview and lint don't.

The repo ships a working example in [`index.html`](index.html) — a 10s, three-scene title card that
passes `npm run lint`. Edit it (and add files under `compositions/`) to make your own video.

## Documentation

- **For agents:** [`CLAUDE.md`](CLAUDE.md) (and [`AGENTS.md`](AGENTS.md) for other agents) — entry
  point and hard rules.
- **For everything about HyperFrames:** the **[`docs/`](docs/README.md)** folder — a distilled,
  English summary of the official docs covering concepts, the full CLI, the authoring model (Rule of
  Three + data attributes), animation, components/registry, templates, shader transitions, the
  design system, the agent workflow, real use cases, and troubleshooting.
- **In the terminal:** `npx hyperframes docs <topic>`.

## Project layout

```
index.html            # main composition (the example title card)
compositions/         # sub-compositions + installed registry components/
assets/               # small INPUT media (large media is gitignored)
out/                  # render OUTPUT (gitignored)
docs/                 # the English summary docs — read these
hyperframes.json      # registry + path config
meta.json             # project metadata
CLAUDE.md / AGENTS.md # agent guides
```

## Output & git policy

**Rendered video output goes to `out/` and is gitignored — it is never committed.** Generated
artifacts (snapshots, beats, transcripts, captured footage, TTS audio, large media) are ignored too;
see [`.gitignore`](.gitignore). Only source — compositions, config, small input assets, and docs —
is committed.

## License / credits

Built on the open-source [HyperFrames](https://github.com/heygen-com/hyperframes) framework by HeyGen
(Apache 2.0). This repo's compositions and docs are your own; HyperFrames is pulled via `npx` at the
version pinned in [`package.json`](package.json).
