# 02 · Getting Started & CLI

## Requirements

- **Node.js 22+**
- **FFmpeg** (+ FFprobe) — required for local rendering only (`brew install ffmpeg` /
  `sudo apt install ffmpeg`). Not needed for `preview` or `lint`.
- **Chrome Headless Shell** — for local rendering: `npx hyperframes browser ensure`.
- Optionally **Docker** — for deterministic `--docker` renders.
- Optionally an AI coding agent (Claude Code, Cursor, Gemini CLI, Codex) for the AI-assisted flow.

Run `npx hyperframes doctor` (or `npm run doctor`) at any time to check your environment.

## Scaffolding a project

```bash
npx hyperframes init my-video        # scaffold a new project
npx hyperframes init . --example title-card   # scaffold into cwd from a template
```

> **This repo is already scaffolded.** `index.html` is a working example. You normally won't run
> `init` here — just edit `index.html` and the files under `compositions/`.

## Install agent skills (one-time)

```bash
npx hyperframes skills            # or: npx skills add heygen-com/hyperframes
```

This installs the HyperFrames + GSAP skills (`/hyperframes`, `/hyperframes-core`, etc.) into your
agent. Restart the agent session afterward.

## The production loop

```
plan → write HTML → wire seekable GSAP animation → add media
     → npm run lint / check → npm run dev (preview) → npm run render
```

Always `npm run check` (lint + validate + inspect) before considering a video done.

## CLI command reference (`hyperframes <command>`)

The npm scripts in this repo wrap the most common ones. Run `npx hyperframes <command> --help` for
full options.

### Getting started
| Command | Purpose |
| --- | --- |
| `init` | Scaffold a new composition project |
| `add <name>` | Install a block or component from the registry (see [05](05-components-registry.md)) |
| `catalog` | Browse and install blocks and components |
| `capture` | Capture a website for video production |
| `preview` | Start the studio for previewing compositions (**long-running**) |
| `present` | Open a slideshow deck in presenter mode |
| `publish` | Upload a project and get a stable public URL |
| `render` | Render a composition to MP4 or WebM |

### Project
| Command | Purpose |
| --- | --- |
| `lint` | Validate a composition for common mistakes |
| `inspect` | Inspect rendered visual layout across the timeline |
| `snapshot` | Capture key frames as PNG screenshots for visual verification |
| `beats` | Detect beats in the music track → `beats/<audio>.json` |
| `info` | Print project metadata |
| `compositions` | List all compositions in the project |
| `docs <topic>` | View inline documentation in the terminal |
| `validate` | Structural validation (run via `npm run check`) |

### Tooling
| Command | Purpose |
| --- | --- |
| `benchmark` | Render preset fps/quality/worker configs and compare speed/size |
| `browser` | Manage the Chrome browser used for rendering (`browser ensure`) |
| `doctor` | Check system dependencies and environment |
| `upgrade` | Check for updates and show upgrade instructions |

### Deploy
| Command | Purpose |
| --- | --- |
| `cloud` | Render on HeyGen's cloud (no local Chrome/ffmpeg) |
| `lambda` | Distributed renders on AWS Lambda |
| `cloudrun` | Distributed renders on Google Cloud Run |

### AI & integrations
| Command | Purpose |
| --- | --- |
| `skills` | Install HyperFrames + GSAP skills for AI coding tools |
| `transcribe` | Audio/video → word-level timestamps (or import a transcript) |
| `tts` | Generate speech from text with a local model (Kokoro-82M) |
| `remove-background` | Make a video/image background transparent |

## Key flags

### `render`
| Flag | Meaning |
| --- | --- |
| `-o, --output <path>` | Output file path (default `out/`) |
| `-f, --fps <24\|30\|60>` | Frame rate (default 30) |
| `-q, --quality <draft\|standard\|high>` | Quality (default standard; use **draft** to iterate fast) |
| `-w, --workers <1-8>` | Parallel capture workers (default auto; **4 is the usual sweet spot**) |
| `--format webm` | Transparent WebM overlay output |
| `--docker` | Deterministic render (pinned Chrome + fonts); needs Docker running |
| `--crf <n>` / `--video-bitrate <e.g. 10M>` | Encoder quality (mutually exclusive) |
| `--gpu` | Hardware FFmpeg encoding (NVENC/VideoToolbox/AMF/VAAPI/QSV) |
| `--browser-gpu` / `--no-browser-gpu` | Force host GPU or software (SwiftShader) for Chrome/WebGL |
| `--video-frame-format <auto\|jpg\|png>` | Source-video frame extraction; use **png** for UI/screen captures |

### `lint`
| Flag | Meaning |
| --- | --- |
| `--verbose` | Include info-level findings |
| `--json` | Machine-readable output (for CI/tooling) |

### `add`
| Flag | Meaning |
| --- | --- |
| `--dir <dir>` | Target project directory |
| `--json` | Machine-readable summary (written files + include snippet) |
| `--no-clipboard` | Don't copy the include snippet to the clipboard |

## In-terminal docs (offline, no network)

```bash
npx hyperframes docs <topic>
# topics: data-attributes, gsap, compositions, rendering, examples, troubleshooting
```

## Full online docs

Discover pages via the machine-readable index — **do not guess URLs**:

```
https://hyperframes.heygen.com/llms.txt
```
