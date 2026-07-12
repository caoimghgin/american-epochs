---
id: AE-3
title: Wheel-zoom + drag-pan input
type: Story
status: Backlog
priority: High
reporter: Kevin Muldoon
assignee: Unassigned
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

- [ ] Scroll/wheel zooms continuously, anchored under the cursor (the year under the pointer stays under the pointer through the zoom)
- [ ] Click-drag pans the timeline smoothly
- [ ] Zooming and panning feel responsive at the current dataset size (no visible jank on a normal re-render)
- [ ] Min/max zoom clamps prevent the view from going degenerate (inverted range, zero-width, or zoomed out past known history)
- [ ] Click-to-snap on an epoch/cycle still works and animates smoothly alongside free pan/zoom

## Non-Goals

- Polished touch/mobile gesture handling beyond basic pinch — revisit later if it's actually rough
- Keyboard-based pan/zoom (nice-to-have, not required here)

## Related

- Epic: [AE-1](AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2
