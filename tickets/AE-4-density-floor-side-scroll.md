---
id: AE-4
title: Density floor + side-scroll for full-history view
type: Story
status: Backlog
priority: Medium
reporter: Kevin Muldoon
assignee: Unassigned
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

- [ ] The full-history default view never renders below the chosen floor density
- [ ] When the full range at floor density exceeds the container width, the chart scrolls horizontally rather than compressing further
- [ ] Existing per-item label suppression remains as a fallback but is no longer the primary answer to crowding at full zoom-out
- [ ] Picking the floor value is a documented, deliberate choice (not an arbitrary leftover), ideally checked against the narrowest real epoch's label

## Non-Goals

- Doesn't attempt to pick a floor that will definitely still work once the pre-1854 epochs (not yet filed) are added — revisit then if needed
- Doesn't remove the label-suppression code entirely, just reduces how often it should trigger

## Related

- Epic: [AE-1](AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2
- Supersedes the squeeze-to-fit rationale behind commits `4f099f8` and `aaed21d`
