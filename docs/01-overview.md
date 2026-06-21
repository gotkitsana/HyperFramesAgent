# 01 · Overview

## What is HyperFrames?

HyperFrames is an **open-source framework (Apache 2.0) that turns HTML, CSS, media, and seekable
animations into deterministic MP4/WebM videos.** Its tagline: *"Write HTML. Render video. Built for
agents."*

A "composition" is just an **HTML file** with a few `data-*` attributes. The same file plays in a
browser and renders to video — there is **no React requirement** and **no proprietary timeline
format**. Because agents already write HTML/CSS/JS, video generation drops naturally into AI coding
workflows.

## Video as code: the deterministic pipeline

HyperFrames renders by driving **headless Chrome** to *seek* an animation timeline to each frame,
capturing pixels, then encoding with **FFmpeg**. It does **not** play animations in wall-clock real
time — it seeks to exact frame times. The guarantee:

> **Same input → same frames → same output.**

That determinism is what makes HyperFrames suitable for CI, regression tests, and automated content
pipelines.

Pipeline stages:

1. **Parse** — the HTML composition is read for `data-*` attributes and registered timeline objects.
2. **Capture** — headless Chrome seeks each animation timeline to a frame and screenshots it.
3. **Encode** — FFmpeg turns captured frames into video.
4. **Audio mix** — separate audio tracks are mixed and synced.
5. **Output** — a final deterministic MP4 (or transparent WebM) file in `out/`.

## Package map

HyperFrames is a monorepo of focused packages:

| Package | Purpose |
| --- | --- |
| `hyperframes` (CLI) | Scaffolding, preview studio, lint/validate/inspect, render, publish |
| `@hyperframes/core` | Types, parsers, linter, runtime, frame adapters |
| `@hyperframes/engine` | Puppeteer-based frame capture + FFmpeg encoding |
| `@hyperframes/producer` | Full pipeline: capture, encode, audio mix |
| `@hyperframes/studio` | Browser-based composition editor / preview |
| `@hyperframes/player` | Embeddable web-component viewer |
| `@hyperframes/shader-transitions` | WebGL transition effects (HyperShader) |
| `@hyperframes/aws-lambda` | Distributed rendering on AWS Lambda |

The CLI also exposes **cloud** rendering (HeyGen cloud — no local Chrome/ffmpeg needed), **lambda**,
and **cloudrun** for distributed renders.

## Frame Adapters

Animations must be **seekable and frame-accurate**. HyperFrames integrates animation runtimes via a
**Frame Adapter** pattern, so you can build motion with:

- **GSAP** (GreenSock) — the default and best-supported
- CSS animations
- Lottie
- Three.js
- Anime.js
- Web Animations API (WAAPI)
- Custom runtime adapters

Adapters expose timelines to the renderer (for GSAP, via `window.__timelines`) so the engine can
seek each one deterministically. See [04 · Animation & Determinism](04-animation-and-determinism.md).

## HyperFrames vs Remotion

| Dimension | HyperFrames | Remotion |
| --- | --- | --- |
| Authoring | HTML + CSS + seekable animation | React components (JSX) |
| Build step | None — `index.html` renders directly | Bundler required |
| Agent handoff | Plain HTML files | JSX/React projects |
| Animation model | Seekable, frame-accurate | Wall-clock patterns (need care) |
| License | Apache 2.0 | Source-available (Remotion License) |

> The core bet: *"Remotion's bet is React components; HyperFrames' bet is plain HTML that humans
> and agents can both write easily."*

## Where to go next

- Install & commands → [02 · Getting Started & CLI](02-getting-started.md)
- The required structure of a composition → [03 · Authoring Model](03-authoring-model.md)
