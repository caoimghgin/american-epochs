---
id: AE-4
title: Density floor + side-scroll for full-history view
type: Story
status: Done
priority: Medium
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [dataviz, rewrite, zoom, layout]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Give the default/full-zoomed-out view a minimum px-per-year (a density floor). If the full known range would fall below that floor at the container's width, render at the floor instead and let the chart exceed the viewport — side-scroll picks up the slack instead of the chart squeezing further.

## Motivation

Matches the explicit behavior in the first continuous-zoom sketch ("anything that doesn't fit in the window is a side scroll"). It's also a real fix rather than a stopgap: the label-suppression logic shipped for the discrete-zoom version (commit `aaed21d` and the pass just before it) exists only because the old full view always squeezed to exactly the container width no matter how many epochs it had to fit. A density floor removes most of the pressure that made suppression necessary in the first place.

## Current State

- Full/no-zoom view sets `W = containerW` unconditionally — the entire known year range always squeezes to fit exactly the container, regardless of how many epochs or how dense the events are.
- Epoch bands, the cycle rail, and event labels each independently compute "would this text fit without overlapping" and skip rendering if not — a binary per-item fallback for the squeeze-to-fit problem.

## Proposed Approach

- Choose a floor px-per-year (needs a real number — informed by the narrowest existing epoch's label width, e.g. enough that a ~6-7 year epoch like The New Freedom or A New Deal can still hold its full name+years).
- Default view width becomes `max(containerW, floor * totalKnownYears)` rather than always exactly `containerW`.
- Once the floor exists, re-evaluate whether the per-item label-suppression fallback is still needed at all, or only for genuinely pathological cases (a very short epoch even at the floor) — it should get exercised far less often than before.

## Acceptance Criteria

- [x] The full-history default view never renders below the chosen floor density
- [x] When the full range at floor density exceeds the container width, the chart scrolls horizontally rather than compressing further
- [x] Existing per-item label suppression remains as a fallback but is no longer the primary answer to crowding at full zoom-out
- [x] Picking the floor value is a documented, deliberate choice (not an arbitrary leftover), ideally checked against the narrowest real epoch's label

## Implementation Notes

- **Architectural adaptation from the original proposal**: the ticket (written before AE-2/AE-3 existed) proposed reintroducing a wider-than-container SVG with native horizontal scroll. Since AE-2/AE-3 already built a continuous `viewStart`/`viewEnd` + real drag-pan, adding a *second*, competing pan mechanism (native scrollbar) would have fought with the first. Instead, the floor is enforced as `maxViewSpan` — a hard cap on how many years the view is ever allowed to show at once (`(containerW - PAD_L - PAD_R) / FLOOR_PX_PER_YEAR`) — and AE-3's existing drag-pan is what reaches whatever falls outside that. Functionally this *is* side-scroll (you must navigate to see content that doesn't fit, never a compressed everything-at-once view); it just reuses the pan mechanism already built rather than introducing `.chart-scroll`'s dormant native scrollbar.
- `FLOOR_PX_PER_YEAR = 22`, chosen against "The New Freedom" (6 years, 16 characters — the narrowest real epoch): its label needs `16*6.4+14 ≈ 116px` (the same fit formula `drawEpochs()` already uses) to render, which is ~19.4px/year; 22 gives headroom without being far above what's actually required.
- The **default/reset view is recency-anchored**, not a symmetric slice of all known history: on first load and on Reset/Escape, the view lands on `[YEAR_MAX - min(fullRange, maxViewSpan), YEAR_MAX]` — the most recent floor-respecting window. This is a deliberate, separate product choice (users almost certainly want to land near "now"), distinct from `clampView()`'s general over-max-span handling, which shrinks unbiased around whatever midpoint was requested (used for click-to-snap targets and wheel-zoom-out, where recency-biasing would be wrong).
- Added a visual affordance (`.edge-fade` divs, CSS gradient, opacity toggled in `render()`) hinting there's more history to pan to whenever `viewStart > YEAR_MIN` or `viewEnd < YEAR_MAX` — otherwise the new default view could silently read as "this is all the history there is," which conflicts with the project's own honesty-in-visualization stance (same reasoning as the crime/RFT lane coverage notes in the sources footer).
- Relabeled the reset button "↺ Reset" (was "↺ Full view") since it may no longer literally show everything.
- **Real bug found and fixed along the way, not introduced fresh here but made far more likely to surface by this story**: `clampView()` previously clamped `start`/`end` independently via `Math.max(YEAR_MIN, start)` / `Math.min(YEAR_MAX, end)`. When a requested range straddled a boundary, this **shrank the span** instead of **sliding the window** — repeated panning at the edge (now a routine action, since the floor forces users to pan to reach older history) compounded this into a runaway collapse toward zero width. Fixed by capping the span first, then sliding both edges together into bounds rather than clamping them independently. Confirmed via Playwright: 10 repeated drags into the earliest history now correctly pin at `[YEAR_MIN, YEAR_MIN + maxViewSpan]` instead of collapsing.
- Verified via Playwright: default view lands on a recent floor-respecting window (not full history) with the left edge-fade visible and right fade hidden; Reset returns to the identical window; dragging into the earliest history correctly pins at the boundary with edge-fades flipping accordingly; wheel-zoom-out from any position clamps to `maxViewSpan` rather than jumping to the full 175-year range; click-to-snap on an off-screen cycle still navigates correctly — zero console errors throughout.

## Non-Goals

- Doesn't attempt to pick a floor that will definitely still work once the pre-1854 epochs (not yet filed) are added — revisit then if needed
- Doesn't remove the label-suppression code entirely, just reduces how often it should trigger

## Related

- Epic: [AE-1](../AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2
- Supersedes the squeeze-to-fit rationale behind commits `4f099f8` and `aaed21d`
