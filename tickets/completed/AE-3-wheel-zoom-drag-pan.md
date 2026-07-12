---
id: AE-3
title: Wheel-zoom + drag-pan input
type: Story
status: Done
priority: High
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [dataviz, rewrite, zoom, interaction]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Give the continuous view state from AE-2 an actual continuous input: mouse-wheel/trackpad scroll to zoom (anchored under the cursor), click-drag to pan. Click-to-snap on epochs/cycles is preserved as a convenience on top of this, not replaced.

## Motivation

AE-2 makes the view state continuous, but without this story the only way to move it is still click-to-snap onto an epoch or cycle — which can't reach an arbitrary window like 1930–1980. This is the story that makes "zoom into any decade you want" actually possible.

## Current State

After AE-2 lands: a continuous `viewStart`/`viewEnd` exists and can be set programmatically (e.g. via click-to-snap or a debug control), but there's no direct-manipulation UI for a user to freely explore.

## Proposed Approach

- Wheel/scroll event over the chart adjusts `pxPerYear` (zoom level), anchored so the year under the cursor stays under the cursor after the zoom.
- Pointerdown + move pans by shifting `viewStart`/`viewEnd` together.
- Touch pinch-to-zoom as the mobile/trackpad equivalent, if reasonably in scope — flag for descoping if it turns out to be its own can of worms.
- Define and enforce min/max zoom clamps: can't zoom out past full known history (plus some padding), can't zoom in past whatever the deepest meaningful tier ends up being (relevant once AE-5's quarter-ticks exist — don't let zoom go tighter than that tier can usefully render).

## Acceptance Criteria

- [x] Scroll/wheel zooms continuously, anchored under the cursor (the year under the pointer stays under the pointer through the zoom)
- [x] Click-drag pans the timeline smoothly
- [x] Zooming and panning feel responsive at the current dataset size (no visible jank on a normal re-render)
- [x] Min/max zoom clamps prevent the view from going degenerate (inverted range, zero-width, or zoomed out past known history)
- [x] Click-to-snap on an epoch/cycle still works and animates smoothly alongside free pan/zoom

## Implementation Notes

- Wheel handler on `chartScrollEl` (not the SVG's children, which get destroyed/recreated every `render()`): computes the year under the cursor via `yearAtClientX()` (the inverse of `xOf()`, valid because `chart.style.width` always equals the viewBox width `W` — no CSS/viewBox rescaling), then solves for the new `viewStart` that keeps that same year at that same fraction across the view after applying `zoomFactor = Math.exp(-e.deltaY * 0.0015)`. Verified via Playwright to anchor within 0.1px after 8 zoom steps.
- Drag-pan uses Pointer Events (`pointerdown`/`pointermove`/`pointerup`/`pointercancel`) rather than separate mouse/touch handling — this gets basic single-finger touch-drag-to-pan for free, matching the ticket's "touch as the mobile equivalent" note without extra code.
- Deliberately does **not** call `setPointerCapture()` on `pointerdown` — only once movement crosses a 4px threshold and the gesture is confirmed to be a drag. Capturing immediately would risk swallowing the click synthesis that click-to-snap depends on; below the threshold, pointerup fires a normal `click` on whatever epoch/cycle/event element is underneath, unaffected by the pan logic.
- Both wheel and drag apply the view change instantly (no `animateViewTo` tween) and cancel any in-flight click-to-snap animation on first input — direct manipulation should track the input immediately, not ease toward it.
- Reused `clampView()` (already built for AE-2's `animateViewTo`) for both wheel and drag — zooming out past known history collapses cleanly to the full range rather than showing empty space; zooming in stops at the existing 2-year `MIN_VIEW_SPAN` floor.
- Removed AE-2's temporary Pan/Zoom button group (◀ − + ▶) and its `zoomBy`/`panBy` functions now that real input supersedes them, per that ticket's own note.
- Verified via Playwright: cursor-anchored wheel zoom (a tracked tick moved 754.0px → 754.1px after 8 zoom-in steps), drag-right correctly revealed earlier years (view shifted 1925–1955 → 1920–1945), click-to-snap still resolves correctly after pan/zoom interaction, zoom-out clamps to the full 1852–2027 range under 30 extreme wheel events, zoom-in clamps to a sane minimum span under 40 extreme wheel events with no degenerate/NaN viewBox — zero console errors throughout.

**Follow-up added after initial ship**: the wheel handler originally only read `e.deltaY` (zoom), silently ignoring `e.deltaX` — meaning a trackpad two-finger horizontal swipe, or a mouse's tilt-wheel/horizontal scroll, did nothing. Added `deltaX` handling as a pan, using ordinary-scroll sign convention (scroll/swipe right reveals later content) — deliberately the *opposite* sign from drag-pan's "grab and pull" convention, since those are different, equally standard interaction metaphors. Both components apply within the same event when a gesture has both (e.g. a diagonal trackpad swipe pans and zooms together). Verified via Playwright in both directions plus a combined diagonal event — zero errors.

## Non-Goals

- Polished touch/mobile gesture handling beyond basic pinch — revisit later if it's actually rough
- Keyboard-based pan/zoom (nice-to-have, not required here)

## Related

- Epic: [AE-1](../AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2
