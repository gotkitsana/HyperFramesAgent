# HyperFrames — Technical Summary Docs

This folder is the **distilled, English source-of-truth** for working with
[HyperFrames](https://hyperframes.heygen.com/) in this repo. It summarizes the official HyperFrames
documentation, the open-source repo (`heygen-com/hyperframes`, Apache 2.0), and the framework's
in-terminal docs (`npx hyperframes docs <topic>`).

**Agents:** when you need to know anything about HyperFrames, read the relevant file here **before**
guessing. `CLAUDE.md` at the repo root points here for a reason.

## Table of contents

| # | Doc | What's inside |
| --- | --- | --- |
| 01 | [Overview](01-overview.md) | What HyperFrames is, "video as code", the deterministic pipeline, package map, HyperFrames vs Remotion |
| 02 | [Getting Started & CLI](02-getting-started.md) | Requirements, install, **every CLI command + key flags**, the production loop, skills |
| 03 | [Authoring Model](03-authoring-model.md) | The **Rule of Three**, full `data-*` attribute reference, composition/clip/track model, sub-compositions, variables |
| 04 | [Animation & Determinism](04-animation-and-determinism.md) | GSAP, frame adapters, `window.__timelines`, seekable vs wall-clock, determinism rules, animation recipes |
| 05 | [Components & Registry](05-components-registry.md) | `hyperframes add` / `catalog`, blocks vs components, tags |
| 06 | [Templates & Skeletons](06-templates-skeletons.md) | Built-in `init --example` templates, skeleton picker, scene durations, aspect ratios |
| 07 | [Shader Transitions](07-shader-transitions.md) | Shader names, the "use sparingly" rule, anchor/non-anchor visibility, energy matching |
| 08 | [Design System](08-design-system.md) | Typography, palette, grain, the `frame.md` / DESIGN.md concept |
| 09 | [Agent Workflow](09-agent-workflow.md) | Claude Design → Claude Code division of labor, the self-review checklist, top mistakes |
| 10 | [Use Cases](10-use-cases.md) | Real adopters (HeyGen, tldraw, TanStack) and the use-case catalog |
| 11 | [Troubleshooting](11-troubleshooting.md) | ffmpeg/Chrome setup, lint failures, render≠preview, Safari/iframe shader bugs |

## Quickest possible start

```bash
npm run dev      # preview (run in background)
npm run check    # lint + validate + inspect
npm run render   # MP4 → out/ (needs ffmpeg + Chrome)
```

The repo already ships a valid example composition in `index.html` (a 10s, three-scene title card)
that passes `npm run lint` — use it as a reference for the Rule of Three.

## Sources

- Official site & docs: <https://hyperframes.heygen.com/> (and the machine-readable index at
  `/llms.txt`)
- Open-source repo: <https://github.com/heygen-com/hyperframes> (Apache 2.0)
- In-terminal reference: `npx hyperframes docs <topic>`
- HeyGen Help Center: "HyperFrames x HeyGen"
- Design guide: `docs/guides/claude-design-hyperframes.md` in the upstream repo

> These docs reflect HyperFrames CLI **v0.6.119** (pinned in `package.json`). Run
> `npx hyperframes upgrade` to check for newer versions, and re-verify flags with
> `npx hyperframes <command> --help`.
