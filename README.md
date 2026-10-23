# Bézier Curves: 1962 Interactive Demo

An interactive demonstration of the curve formulation **Pierre Bézier published in
1962** at Renault — presented as the instrument panel that might have run it: a
riveted steel cabinet, indicator lamps, an X–Y plotter on millimetre paper, lever
switches, and a mechanical odometer for the parameter *t*.

Drag the control points and the pen follows. Everything is live: the de Casteljau
construction ladder, the Bernstein weights, the basis-function chart, and the
coordinate readout all recompute as you work.

## Why 1962

The mathematics was worked out twice, in secret, by two rival French car makers:

- **1959 — Paul de Casteljau (Citroën)** derives the recursive construction now
  called *de Casteljau's algorithm*. Citroën classifies it; he cannot publish
  until 1974.
- **1962 — Pierre Bézier (Renault)** independently develops the equivalent
  formulation in terms of Bernstein polynomials and **publishes it** — which is
  why the curves carry his name. It becomes the heart of Renault's **UNISURF**
  CAD/CAM system for car-body design.

This app demonstrates that 1962 publication, dressed in the UI/UX paradigms of
its era: no menus, no dialogs — lamps, levers, faders, and typed readouts.
See [`docs/HISTORY.md`](docs/HISTORY.md) for the full story.

## The control desk

| Section | What it does |
| --- | --- |
| **§ 1 — Table traçante** (plotter) | The curve on millimetre paper. Drag any control point P₀…Pₙ to reshape it. |
| **Paramètre t** | Fader + odometer counter for *t* ∈ [0, 1]. **▶ Lecture** plays the sweep, **Pas +** steps, **R.A.Z.** resets. |
| **Levers (OUI/NON)** | Toggle the control polygon, the de Casteljau construction, the grid paper, and the convex hull. |
| **Degré II–V** | Set the curve degree (2–5); **+ Point / − Point** add or remove a control point. |
| **Programmes** | Preset control polygons — *Carrosserie*, *Esse*, *Arc*, *Boucle* — loaded like programme cards. |
| **Relevé** | Live coordinate table of every control point and of the current point B(t). |
| **Bernstein weights** | The formula B(t) = Σ Bᵢ,ₙ(t)·Pᵢ with a live weight bar per term, plus a chart of the basis functions Bᵢ,ₙ(t) for the current degree. |
| **Dossier** | A typed briefing on Paris 1959–1962: the race to draw by numbers. |

## Running it

There is no build step and nothing to install — the whole application is one
self-contained `index.html`.

```sh
# Option 1: just open it
open index.html            # macOS
xdg-open index.html        # Linux

# Option 2: serve it locally
python3 -m http.server 8000
# then visit http://localhost:8000
```

Fonts (Courier Prime, IBM Plex Mono, Libre Franklin) load from Google Fonts;
offline, the page falls back to system monospace — which suits the period
aesthetic anyway.

## Repository layout

```
.
├── index.html          # the entire application (markup, styles, engine)
├── assets/
│   └── icon.webp       # application icon
└── docs/
    ├── HISTORY.md      # de Casteljau, Bézier, and UNISURF — the 1959–1962 story
    └── ARCHITECTURE.md # how index.html is organised, section by section
```

The commit history is deliberately batched and topological — scaffold first,
then the panel shell and stylesheet, the control-desk markup, the mathematics
engine, assets, and finally the documentation — so `git log --oneline` reads
like the assembly order of the machine. See
[`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for details.

## The mathematics in one paragraph

A Bézier curve of degree *n* is B(t) = Σᵢ Bᵢ,ₙ(t) Pᵢ for t ∈ [0, 1], where
Bᵢ,ₙ(t) = C(n, i) (1 − t)ⁿ⁻ⁱ tⁱ are the Bernstein basis polynomials. De
Casteljau's construction reaches the same point by repeated linear
interpolation between neighbouring control points — the ladder of lines drawn
on the plotter. Both methods are implemented side by side in the app's engine,
which is a fitting tribute: the two men found the same curve from opposite
directions.
