# 03 · Authoring Model

A **composition** is an HTML document that defines a video timeline. Everything timed is expressed
with `data-*` attributes; motion is layered on top with a registered (paused) GSAP timeline.

## The Rule of Three

Three structural contracts must always hold:

1. **Root** element declares the composition:
   `data-composition-id` **+** `data-width` **+** `data-height`.
2. **Every timed element** is `class="clip"` **+** `data-start` **+** `data-duration` **+**
   `data-track-index`. The `clip` class is how the runtime manages an element's visibility
   lifecycle — **timed elements without it will not show/hide correctly.**
3. **GSAP timelines** are created with `{ paused: true }` and registered on
   `window.__timelines["<composition-id>"]`. The key must match the root's `data-composition-id`.

If any of these are missing, `npx hyperframes lint` will flag it.

## Data attribute reference

### Timing
| Attribute | Meaning |
| --- | --- |
| `data-start="0"` | Start time, in **seconds** |
| `data-duration="5"` | Duration, in seconds |
| `data-track-index="0"` | Timeline track number — controls **z-ordering / layering** |

### Media
| Attribute | Meaning |
| --- | --- |
| `data-media-start="2"` | Media playback offset / trim point (seconds) |
| `data-volume="0.8"` | Audio/video volume, `0`–`1` |
| `data-has-audio="true"` | Marks that a video carries an audio track |

### Composition
| Attribute | Meaning |
| --- | --- |
| `data-composition-id="root"` | **Required** unique ID for the composition wrapper |
| `data-width="1920"` | Composition width (px) |
| `data-height="1080"` | Composition height (px) |
| `data-composition-src="./intro.html"` | Source for a **nested** composition |

### Visibility
- Add `class="clip"` to any timed element so the runtime can manage its visibility lifecycle.

## Minimal composition

```html
<div id="root" data-composition-id="root" data-width="1920" data-height="1080">
  <!-- timed elements go here -->
</div>
```

## Annotated example (clips, tracks, audio)

```html
<div id="stage" data-composition-id="launch"
     data-start="0" data-duration="6" data-width="1920" data-height="1080">

  <video class="clip" data-start="0" data-duration="6" data-track-index="0"
         src="intro.mp4" muted playsinline></video>

  <h1 id="title" class="clip" data-start="1" data-duration="4" data-track-index="1">
    Launch day
  </h1>

  <audio data-start="0" data-duration="6" data-track-index="2"
         data-volume="0.5" src="music.wav"></audio>
</div>
```

- **Tracks** (`data-track-index`) stack like layers: higher index renders on top.
- **Video** elements use `muted playsinline`; route real audio through a separate `<audio>` clip.
- The window of every clip is `[data-start, data-start + data-duration)`. Keep scenes tiled
  end-to-end (no accidental gaps/overlaps) unless you intend an overlap.

> The repo's `index.html` is a working three-scene example following all of the above — read it
> alongside this doc.

## Sub-compositions (nesting)

Embed one composition inside another by referencing its file:

```html
<div data-composition-src="./compositions/intro.html"
     data-start="0" data-duration="5"></div>
```

List every composition in the project with `npx hyperframes compositions`.

## Variables (parameterized compositions)

Two related attributes with different jobs:

- **`data-composition-variables`** on the `<html>` root — a JSON **array of declarations**
  (`{id, type, label, default}` per entry). Defines *which* variables exist and their defaults.
- **`data-variable-values`** on a sub-comp host element — a JSON **object keyed by variable id**
  (`{"title":"Pro","price":"$29"}`). Per-instance overrides for that one mount.

Inside a composition script, `window.__hyperframes.getVariables()` returns the merged result.
Precedence, lowest → highest:

1. Declared defaults (`data-composition-variables`)
2. Per-instance overrides (`data-variable-values` on the embed)
3. CLI overrides (`npx hyperframes render --variables '{...}'`, top-level renders only)

```html
<!-- compositions/card.html -->
<html data-composition-variables='[
  {"id":"title","type":"string","label":"Title","default":"Hello"},
  {"id":"color","type":"color","label":"Color","default":"#111827"}
]'>
  <body>
    <div data-composition-id="card" data-width="1920" data-height="1080">
      <h1 class="title"></h1>
      <script>
        const { title, color } = window.__hyperframes.getVariables();
        const h = document.querySelector(".title");
        h.textContent = title;
        h.style.color = color;
      </script>
    </div>
  </body>
</html>

<!-- index.html — embed twice with different values -->
<div data-composition-id="card-pro" data-composition-src="compositions/card.html"
     data-variable-values='{"title":"Pro","color":"#ff4d4f"}'></div>
<div data-composition-id="card-enterprise" data-composition-src="compositions/card.html"
     data-variable-values='{"title":"Enterprise","color":"#22c55e"}'></div>
```

This makes templated, data-driven videos possible (one composition, many renders).
