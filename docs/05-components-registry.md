# 05 · Components & Registry

HyperFrames ships a **registry** of reusable building blocks you can drop into a composition. The
registry URL is configured in `hyperframes.json`:

```json
{
  "registry": "https://raw.githubusercontent.com/heygen-com/hyperframes/main/registry",
  "paths": {
    "blocks": "compositions",
    "components": "compositions/components",
    "assets": "assets"
  }
}
```

## Installing items

```bash
npx hyperframes add <name>        # install a single block/component
npx hyperframes catalog           # browse & install interactively
```

- If `<name>` is a **single item**, it installs directly (e.g. `shader-wipe`).
- If `<name>` matches a **tag**, all blocks with that tag are installed (e.g. `captions`,
  `html-in-canvas`).
- `add` copies the files into your project (`compositions/` for blocks, `compositions/components/`
  for components) and copies an **include snippet** to your clipboard.

### Useful flags
| Flag | Meaning |
| --- | --- |
| `--dir <dir>` | Target project directory (default: cwd) |
| `--json` | Machine-readable summary (written files + snippet) — handy for agents |
| `--no-clipboard` | Don't copy the include snippet |

## Blocks vs components

- **Blocks** are larger, self-contained composition pieces (a full scene, an overlay, a transition).
- **Components** are smaller reusable parts placed under `compositions/components/`.

## Example registry items

These appear in HyperFrames' registry / docs (browse the live set with `npx hyperframes catalog`):

| Name | Type | What it is |
| --- | --- | --- |
| `flash-through-white` | shader transition | Bright flash cut between scenes |
| `shader-wipe` | shader transition | WebGL wipe between scenes |
| `instagram-follow` | social overlay | Animated IG follow/notification overlay |
| `data-chart` | block | Animated chart / data viz |
| `captions` (tag) | block group | Caption / subtitle blocks |
| `html-in-canvas` (tag) | block group | Render HTML inside a canvas for effects |

> The registry evolves — **always confirm the current catalog** with
> `npx hyperframes catalog` rather than assuming a name exists. Shader transition **effects**
> (separate from installable blocks) are covered in [07 · Shader Transitions](07-shader-transitions.md).

## After installing

1. Paste the include snippet where the block should appear in your composition.
2. Wire any timing (`data-start` / `data-duration`) and fill in content/variables.
3. Run `npm run check` — installed blocks are pre-built to lint clean, but your wiring must too.
