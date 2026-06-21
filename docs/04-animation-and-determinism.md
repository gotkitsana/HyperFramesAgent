# 04 · Animation & Determinism

## How animation works

HyperFrames renders by **seeking** an animation timeline to each frame time and capturing pixels —
not by playing in real time. Animations must therefore be **seekable and frame-accurate**. The
engine reaches your animation through a **Frame Adapter**; for GSAP (the default) it controls
`window.__timelines`.

## GSAP setup

```html
<script src="https://cdn.jsdelivr.net/npm/gsap@3/dist/gsap.min.js"></script>
<script>
  const tl = gsap.timeline({ paused: true });
  tl.to("#title", { opacity: 1, duration: 0.5 }, 0);

  window.__timelines = window.__timelines || {};
  window.__timelines["root"] = tl;   // key === composition-id
</script>
```

### GSAP rules
- Always create timelines with **`{ paused: true }`** — the runtime drives playback by seeking.
- Register on `window.__timelines` keyed by the **composition ID**.
- The **3rd argument is an absolute position in seconds**: `tl.to(el, vars, 1.5)` starts at 1.5s.
- Supported methods: `set`, `to`, `from`, `fromTo`.
- Commonly supported properties: `opacity`, `autoAlpha`, `x`, `y`, `scale`, `scaleX`, `scaleY`,
  `rotation`, `width`, `height`, `visibility`.

`autoAlpha` is `opacity` + `visibility` together — prefer it for show/hide so an element can't be
"visible but transparent" (or vice-versa), which matters when shader transitions blank scenes.

## Determinism — the hard rules

Renders must be reproducible. **Never** use:

- `Math.random()` — use hard-coded values or seeded constants.
- `Date.now()` / `new Date()` — no wall-clock dependence.
- `setInterval` / `setTimeout`-driven motion — drive everything from the timeline.
- `repeat: -1` (infinite loops) — use finite repeats or timeline callbacks.
- Network fetches at render time — assets must be local/embedded.

If motion isn't expressible as a timeline tween, compute it deterministically inside an
`onUpdate` callback tied to a tween's progress (see the counter recipe below).

## Animation recipes

**Count-up (deterministic, no setInterval):**
```js
const counter = { v: 0 };
tl.to(counter, {
  v: 60, duration: 1.8, ease: "power2.out",
  onUpdate: () => { document.querySelector("#stat").textContent = Math.round(counter.v); }
}, 4.2);
```

**Breathing float (finite, not `repeat: -1`):**
```js
tl.to(el, { y: -5, duration: 1.5, ease: "sine.inOut", yoyo: true, repeat: 1 }, t);
```

**Bar-chart fill (stagger):**
```js
tl.from(".bar", { scaleY: 0, transformOrigin: "bottom", stagger: 0.08, duration: 0.6 }, t);
```

**SVG stroke draw:**
```js
tl.fromTo("#path", { strokeDashoffset: LENGTH }, { strokeDashoffset: 0, duration: 1.2 }, t);
```

**Character stagger (kinetic type):**
```js
tl.from(".char", { y: 60, autoAlpha: 0, stagger: 0.12, ease: "power3.out" }, t);
```

## Per-scene animation baseline

Aim for these so scenes feel like video, not slides:

1. **Entrance tween** offset ~0.1–0.3s into the scene (avoid jump cuts).
2. **At least one mid-scene activity** for scenes longer than ~4s — a counter, float, glow,
   bar-fill, or slow zoom. *A still element on a still background is a JPEG, not a video.*
3. **Varied eases** — use 3+ different eases per scene; don't default `power2.out` everywhere
   (`expo.out`, `power3.out`, `sine.inOut`, `back.out` etc.).

## Other frame adapters

GSAP is the default, but the adapter pattern also supports **CSS animations, Lottie, Three.js,
Anime.js, and the Web Animations API (WAAPI)**, plus custom adapters. The same determinism rules
apply — the runtime must be able to seek the animation to an exact frame.
