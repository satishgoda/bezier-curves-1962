# Historical notes: Paris, 1959–1962

The story behind the curves this app demonstrates.

## Two rivals, one curve

In the late 1950s, France's two great car makers faced the same problem: car
bodies were sculpted in clay, full-scale, by hand — and translating those
shapes into stamping tools was slow, expensive, and imprecise. Both companies
independently set mathematicians to work on defining smooth shapes *by
numbers*, so a computer could drive the milling machines.

**Citroën — Paul de Faget de Casteljau (from 1958).**
De Casteljau, a physicist-turned-mathematician, devised an elegant recursive
construction: repeatedly interpolate between neighbouring points of a control
polygon until a single point remains — that point lies on the curve. It is now
known as **de Casteljau's algorithm**, and it is the ladder of construction
lines this app draws on the plotter. But Citroën treated the work as a trade
secret. De Casteljau was not permitted to publish; his results only entered
the open literature in **1974**, and Wolfgang Boehm was the first to credit
him publicly, in the late 1970s.

**Renault — Pierre Étienne Bézier (early 1960s).**
Bézier, heading Renault's design department, took a different mathematical
route — through polynomial formulations that turned out to be expressible in
terms of **Bernstein basis polynomials** (Sergei Bernstein, 1912). Crucially,
Bézier **published**: his first descriptions appeared in **1962**, making his
the first published formulation of the technique and attaching his name to
the curves forever. His system grew into **UNISURF**, one of the founding
CAD/CAM systems of industrial design, used to define Renault body surfaces
from clay-model measurements and drive production tooling.

A. R. Forrest later showed the two approaches were mathematically identical —
the same curve, discovered twice, from opposite directions.

## Timeline

| Year | Event |
| --- | --- |
| 1912 | Sergei Bernstein publishes the Bernstein polynomials — pure mathematics, decades before their application. |
| 1958–59 | De Casteljau joins Citroën and derives his recursive construction. It is classified as a company secret. |
| 1962 | Bézier publishes his formulation at Renault — **the first published 3D modelling technique of its kind**. |
| 1963 | Ivan Sutherland's *Sketchpad* (MIT) demonstrates interactive computer graphics and wireframe modelling. |
| 1971 | Bézier's account of the UNISURF system appears, spreading the method through industry. |
| 1974 | Citroën finally allows de Casteljau to publish his 1959 work. |
| 1975 | Martin Newell models the Utah Teapot from Bézier patches — the curve becomes computer graphics' most famous test object. |

## Why "first published" matters

De Casteljau was first in time; Bézier was first in print. Scientific credit
follows publication, and because Renault published while Citroën kept
secrecy, the world learned the technique as **Bézier curves** — even though
the construction most software actually evaluates, step by step, is de
Casteljau's. This app honours both: Bézier's formula drives the weight bars
and the basis chart, while de Casteljau's ladder is drawn on the plotter.
