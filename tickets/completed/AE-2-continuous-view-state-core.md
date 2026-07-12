---
id: AE-2
title: Continuous view-state core
type: Story
status: Done
priority: High
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [dataviz, rewrite, zoom]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Replace the discrete `zoomTarget` (`null` / `{type:'cycle', id}` / `{type:'epoch', id}`) with a continuous view state — `viewStart`/`viewEnd` (or an equivalent `center` + `pxPerYear` pair) representing exactly what date range is currently visible, unconstrained to any epoch or cycle boundary. Everything else in AE-1 depends on this landing first.

## Motivation

The whole point of the rebuild is that a viewer should be able to land on an arbitrary window — 1930–1980, say, cutting through the middle of a cycle and not aligned to any epoch edge — not just jump between three pre-set states. That requires the view itself to be a continuous range, not a reference to a named cycle or epoch.

## Current State

- `zoomTarget` is `null | {type:'cycle', id} | {type:'epoch', id}`.
- `render()` computes `W` (SVG pixel width) from `targetSpan(zoomTarget)` when set, else `W = containerW`.
- `targetSpan()`/`cycleSpan()`/`epochSpan()` derive a `{start, end}` year range from whichever epoch or cycle is targeted.
- Clicking an epoch or cycle band sets `zoomTarget` and re-renders; clicking again (or the back button, or Escape) steps back out one level via `backTarget()`.
- Downstream draw functions (`drawEpochs`, `drawCycles`, `drawEvents`) use `zoomTarget` only to decide ring-highlight state (is *this* epoch/cycle the current target) — they don't otherwise care how the current width was chosen.

## Proposed Approach

- Introduce `viewStart`/`viewEnd` as the source of truth for what's on screen; `xOf()` and `render()`'s width math read from these instead of `targetSpan(zoomTarget)`.
- Keep click-to-zoom on epochs/cycles as a convenience: clicking sets `viewStart`/`viewEnd` to that span's bounds (with the existing ~12% padding convention) and animates the transition, rather than an instant jump.
- Ring-highlight logic in `drawEpochs`/`drawCycles` changes from "does this equal the zoom target" to something like "is this epoch/cycle fully or mostly contained in the current view" — needs a concrete rule, since a continuous view won't usually match a boundary exactly.
- Because this story ships before AE-3 (real wheel/drag input), include some minimal way to manually drive `viewStart`/`viewEnd` to arbitrary non-aligned windows for testing — even a temporary debug control — so this story is verifiable on its own rather than only through click-to-snap.

## Acceptance Criteria

- [x] `viewStart`/`viewEnd` (or equivalent) drives `xOf()`/render width instead of `zoomTarget`
- [x] Clicking a cycle or epoch animates the view to its bounds rather than jumping instantly
- [x] The view can be set to an arbitrary window not aligned to any epoch/cycle boundary, and everything renders correctly there (axis, epoch bands, event dots, overlays)
- [x] Detail panel, tooltips, register toggles, and statistical overlay toggles all continue to work correctly at arbitrary view windows
- [x] Ring-highlight / "current selection" indication on epoch and cycle bands has a defined rule under continuous view (not just an exact-match check)

## Implementation Notes

- `selected` (already existed, for the detail panel) and view state are now fully decoupled: `selected = {type:'epoch'|'cycle', id}` drives ring-highlight and the detail panel; `viewStart`/`viewEnd` drives what's on screen. The "concrete rule" the ticket called for: an epoch's ring shows when `selected` matches it exactly; a cycle's ring shows only when the cycle *itself* is selected, but its box still draws in a heavier ink stroke (no red ring) when a *child epoch* of that cycle is selected — so zooming into an epoch still visually shows which cycle it belongs to.
- `animateViewTo(start, end, duration)` is the one path (besides the temporary debug rig) that ever changes `viewStart`/`viewEnd` — a `requestAnimationFrame` tween with `easeInOutCubic`, clamped to `[YEAR_MIN, YEAR_MAX]` and a 2-year minimum span so the view can't collapse to zero width.
- Click-to-zoom is now a toggle: clicking an already-selected epoch/cycle calls `resetView()` (back to full history) rather than requiring a separate back button/stack — continuous zoom removes the need for the old discrete back/forward levels.
- Added a temporary Pan/Zoom control group (◀ − + ▶ buttons, clearly labeled "temp" in the UI) so this story is testable without waiting on AE-3's real wheel/drag input — verified reaching a genuinely arbitrary window (e.g. 1949–1971, cutting through the middle of The Affluent Society/Unravelling boundary) renders correctly. Remove this control group once AE-3 ships.
- Found and fixed several spots that anchored to `xOf(YEAR_MIN)`/`xOf(YEAR_MAX)` as a stand-in for "left/right edge of the viewport" (axis spine, lane tags, baseline lines) — these silently broke once the view no longer always spans the full known range, and now use `PAD_L`/`W-PAD_R` directly.
- Added a view-bounds cull to `drawEvents()` (skip events well outside `[viewStart, viewEnd]` before row-packing) — not required by the acceptance criteria, but cheap now and will matter once AE-3 triggers many renders per second during a drag.
- Verified via Playwright: initial full-history ticks, cycle-click zoom, epoch-click zoom (with ring), an arbitrary non-aligned window via the temp rig, lane-toggle interaction at that arbitrary window, and Escape-to-reset — zero console errors throughout.

## Non-Goals

- Mouse-wheel zoom / drag-pan input — AE-3
- Density floor + side-scroll for the default view — AE-4
- Quarter-year axis tier — AE-5
- Any change to which events show at which zoom depth — AE-6

## Related

- Epic: [AE-1](../AE-1-continuous-zoom-rebuild.md)
- Blocks: AE-3, AE-4, AE-5
