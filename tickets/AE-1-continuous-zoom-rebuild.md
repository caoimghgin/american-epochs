---
id: AE-1
title: Continuous zoom rebuild — flexible-scale timeline navigation
type: Epic
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-11
labels: [dataviz, rewrite, zoom, ux]
components: [src/american-epochs.html]
---

## Summary

Replace the current three discrete zoom states (all history / one cycle / one epoch) with continuous scale-and-pan navigation — smoothly zooming to any arbitrary point in time, not just jumping between pre-set boundaries — with a ChronoZoom-style ruler that continuously relabels itself as the scale changes, and a priority-based label-declutter system in place of the current binary "does it fit" checks.

## Motivation

Prompted by comparing this instrument to Microsoft Research's ChronoZoom, whose axis ("flexible measuring tape") relabels its units continuously as you zoom rather than snapping between fixed resolutions — that's a genuinely better navigation model for a history timeline than jumping between fixed states, and it would let a viewer zoom into, say, 1965–1966 specifically without that span needing to already be a named epoch or cycle boundary.

The concept worth adopting is continuous/semantic zoom. The literal tile-fetching infrastructure Google Maps and ChronoZoom use for planet- or cosmos-scale datasets is explicitly **not** needed here — see Non-Goals.

## Current State

- Three navigation levels via a single `zoomTarget` (`null` / `{type:'cycle', id}` / `{type:'epoch', id}`), each expanding the SVG width so that span fills the container and scrolling to it (`render()`, `targetSpan()`/`cycleSpan()`/`epochSpan()`).
- `drawAxis()` has exactly two resolution modes: decade-only ticks when zoomed out (`pxPerYear < 15`), every-year ticks with quinquennial/decade emphasis when zoomed in — a hard switch, not a continuum.
- Label density is handled by binary suppression, added in the AE data-expansion pass (commit `aaed21d` and the epoch/event label fixes just before it): epoch names, cycle-rail names, and event labels each compute "would this text fit without overlapping" and either draw or skip — no partial/graceful degradation, no priority ordering (e.g., a tier-1 structural pivot has no better odds of keeping its label than a tier-3 event once its neighborhood is crowded).
- Whole dataset (14 epochs, 3 cycles, ~150 events, presidents back to 1853, NBER recessions back to 1854) is one set of JS arrays loaded with the page — no pagination or fetch-on-demand, and at this size none is needed.

## Proposed Approach

Not fully speced — this is deliberately an Epic to be broken into Stories once picked up. Rough shape:

1. Replace fixed `W`/discrete zoom with a continuous scale factor (px-per-year) driven by scroll/pinch/drag, likely with momentum/easing rather than instant jumps.
2. Rebuild `drawAxis()` as a continuous ruler: tick spacing and label granularity (century → decade → year → month, as scale increases) computed from the current scale rather than switched between two hardcoded modes.
3. Replace the per-layer binary fit checks (epoch bands, cycle rail, event dot-cloud) with a shared priority/collision system: tier-1 events and wider epochs should hold their labels longer than tier-3 events and narrow epochs as things get crowded, rather than each layer solving its own overlap problem independently.
4. Decide whether cycle/epoch "click to zoom to exactly this span" is still a first-class interaction alongside free continuous panning, or whether it becomes a convenience shortcut (e.g., double-click an epoch to snap-zoom to its bounds) layered on top of free navigation.

## Acceptance Criteria

- [ ] Zooming/panning is continuous across the full 1854–present range, not confined to jumping between the three current states
- [ ] Axis relabels itself appropriately at every scale, not just two fixed resolutions
- [ ] Label suppression is priority-ordered (structural pivots and wider epochs outlast minor events and narrow epochs under crowding), not a flat per-item fits/doesn't-fit check
- [ ] Existing features survive the rebuild unchanged in behavior: register system (factual/structural/argued), detail panel on click, statistical overlay toggles, cycle/epoch selection highlighting

## Non-Goals

- **No tile-fetching/quadtree infrastructure.** The dataset (currently ~150 events, a few hundred KB) fits entirely in memory and is small enough that this will remain true even after the pre-1854 epochs (AE-2, not yet filed) are added. Building real map-tile-style dynamic loading would be solving a data-scale problem this project doesn't have.
- **No change to the underlying epoch/cycle framework or data model** (`EPOCHS`/`CYCLES`/`EVENTS` shape) — this is a rendering and interaction rebuild, not a content or schema change.

## Related

- Discrete cycle-zoom tier + label-suppression fixes: commits `349cd46` (v3 epoch zoom), `4f099f8` (v4 data expansion + cycle zoom + label suppression), `aaed21d` (cycle-rail label suppression)
- [ChronoZoom — Microsoft Research](https://www.microsoft.com/en-us/research/project/chronozoom/)
- [GitHub - ChronoZoom (open source)](https://github.com/alterm4nn/ChronoZoom)
