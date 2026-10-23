# Architecture

The entire application is a single self-contained `index.html` (~750 lines,
no dependencies, no build step). It is organised in the three strata a 1962
instrument would suggest: the cabinet (styles), the control desk (markup),
and the mechanism (script). The commit history mirrors these strata.

## 1. The cabinet — `<style>`

The stylesheet builds the physical fiction: a riveted steel cabinet with
corner screws, an engraved nameplate, indicator lamps, chunky hardware
buttons, and lever switches. Type is period monospace (Courier Prime /
IBM Plex Mono, with system-monospace fallbacks). The plotter bed is styled
as millimetre graph paper. Colour respects `prefers-color-scheme`; the
script's `palette()` function reads the same preference so canvas drawing
and CSS never disagree.

## 2. The control desk — `<body>`

| Block | Key elements |
| --- | --- |
| Status strip | Lamps `#lampPower`, `#lampPlay`; readouts `#playStatus`, `#degStatus`, `#tStatus`, `#ptStatus` |
| § 1 Plotter | `<canvas id="plot">` (880×660) — curve, control polygon, construction ladder, hull |
| Parameter desk | Odometer digits `#odoInt/#odoD1/#odoD2`, fader `#tSlider`, `#playBtn` (Lecture), `#stepBtn` (Pas +), `#resetTBtn` (R.A.Z.) |
| Levers | `#swPoly`, `#swCons`, `#swGrid`, `#swHull` — `aria-pressed` toggle switches |
| Degree desk | Degree II–V selector, `#addBtn` / `#removeBtn` for control points |
| Relevé | Coordinate table body `#coordBody` |
| Weights | `#weightRow` — one live Bernstein weight bar per term |
| Basis chart | `<canvas id="basis">` (640×360) + `#basisLegend` |
| Dossier | Typed historical briefing with `#tryBtn` to start the Lecture from the text |

## 3. The mechanism — `<script>`

Plain, dependency-free JavaScript in three layers:

**Mathematics** — pure functions, no DOM:

- `comb(n, k)` — binomial coefficients
- `bernstein(n, i, t)` — the Bernstein basis polynomial Bᵢ,ₙ(t)
- `bezierPoint(pts, t)` — evaluates the curve from the weighted sum
- `deCasteljau(pts, t)` — returns every rung of the interpolation ladder,
  so the plotter can draw the construction, not just the final point
- `hull(pts)` — monotone-chain convex hull of the control points

**Geometry** — the mapping between curve space ([0, 1]², y-up) and the
plotter bed (pixels, y-down): `geom()`, `toPx()`, `fromPx()`, `clamp01()`.

**Rendering & interaction** — a single `renderAll()` fans out to
`drawPlot()`, `drawBasis()`, and `renderTables()`, so every control —
fader, lever, drag, preset — mutates state and re-renders through one path.
Pointer handling (`evtPos`, `hitIndex`, drag lifecycle) makes the control
points draggable on the plotter. The **Lecture** animation is a
`requestAnimationFrame` loop (`startPlay` / `tick` / `stopPlay`) that sweeps
*t*, keeping the odometer, status strip, and weight bars in sync. Switches
are bound through `bindSwitch(id, fn)`; `setDegree(n)` rebuilds the point
set for degrees II–V.

## Design invariants

- **One file, zero build.** What you clone is what you open.
- **Both 1962 methods, side by side.** The formula (Bézier/Bernstein) and
  the construction (de Casteljau) are computed independently and agree —
  which is the historical point of the piece.
- **State flows one way.** Controls mutate a small state object; rendering
  is a pure function of that state.
