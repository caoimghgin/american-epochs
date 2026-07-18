---
id: AE-12
title: Mobile pinch-to-zoom / touch gesture support
type: Story
status: Backlog
priority: Medium
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-14
labels: [mobile, ux, zoom]
components: [src/american-epochs.html]
---

## Summary

On a touchscreen, there is currently no way to zoom the timeline at all — only
single-finger drag-to-pan works. Add pinch-to-zoom (and/or another
touch-native zoom gesture) so mobile visitors can actually reach the
zoom depths where event labels and finer axis detail become legible.

## Motivation

Raised directly: "the mobile view should allow for pinch/zoom or we can't
see things." Confirmed by reading the current input code rather than
assuming — the *only* zoom trigger in the whole instrument is a `wheel`
event listener, which never fires from a touchscreen. Combined with AE-11
removing the manual Depth buttons (an incidental fallback a mobile user
could previously tap instead of scrolling a wheel), mobile visitors are
now left with zero way to zoom in at all, only pan.

## Current State

- `chartScrollEl` has `pointerdown`/`pointermove` listeners for drag-to-pan
  (`PAN_THRESHOLD`-gated so a tap still registers as a click) — these fire
  fine on touch, since Pointer Events unify mouse/touch/pen for single-point
  input.
- Zoom is driven entirely by a single `wheel` event listener reading
  `deltaY` (zoom) and `deltaX` (pan) — see AE-3. There is no multi-touch
  handling anywhere in the file, and no on-screen zoom button of any kind
  to fall back on since AE-11 removed the Depth control group.
- The `<meta name="viewport">` tag is already present and sets
  `width=device-width, initial-scale=1`, so the page itself isn't
  pinch-zoomable at the *browser* level either in a way that would help —
  the SVG interaction needs its own gesture handling.

## Proposed Approach

Rough sketch, not a spec:

- Track two simultaneous `pointerdown` touch points on `chartScrollEl`;
  on `pointermove` with both still down, compute the change in distance
  between them and translate that into the same `viewStart`/`viewEnd`
  zoom math the `wheel` handler already uses, so both input paths converge
  on one shared zoom function rather than duplicating the clamp/animate logic.
  Anchor the zoom on the pinch's midpoint, the same way the wheel handler
  anchors on the cursor position.
- Respect the existing `minViewSpan`/`maxViewSpan` clamps (AE-4/AE-5) and
  `MAX_ZOOM_SCALE` cap — this is a new input path into the same view-state
  core (AE-2), not a new zoom model.
- Decide whether a double-tap-to-zoom-in gesture is worth adding alongside
  pinch, given how fussy precise pinch gestures can be on a small screen —
  not required, but worth a look once pinch itself works.

## Acceptance Criteria

- [ ] Two-finger pinch zooms the timeline in/out on a real touch device
      (or a touch-emulated browser devtools session, at minimum)
- [ ] Pinch respects the same min/max zoom clamps as wheel-zoom
- [ ] Single-finger drag-to-pan continues to work unchanged
- [ ] Verified on at least one real mobile viewport size, not just desktop
      devtools touch emulation

## Non-Goals

- Doesn't redesign the mobile layout/controls bar more broadly — this is
  specifically about restoring the ability to zoom, not a full mobile pass
- Doesn't add a fallback on-screen zoom button unless pinch itself proves
  insufficient after real testing

## Related

- Depends on the view-state core from [AE-2](completed/AE-2-continuous-view-state-core.md)
- Parallel input path to the wheel-zoom handler from [AE-3](completed/AE-3-wheel-zoom-drag-pan.md)
- Made more urgent by [AE-11](completed/AE-11-zoom-driven-progressive-reveal.md) removing the Depth buttons, which had incidentally served as a tap-friendly zoom fallback on mobile
