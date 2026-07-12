---
id: AE-5
title: Quarter-year hashmark axis tier
type: Story
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-12
labels: [dataviz, rewrite, zoom, axis]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Add a new, deepest tier to `drawAxis()`: once px-per-year crosses a high threshold, show four unlabeled hashmarks between each year tick, representing quarters.

## Motivation

Sketched directly during design discussion: "there should be at least one more level down where the pips are sequential for a year... with four hashmarks in between [years] to represent the quarters." Confirms the axis should keep getting more granular the deeper you zoom, past the year-tick level that's currently the finest tier.

## Current State

`drawAxis()` currently has exactly two modes, switched on a single `pxPerYear < 15` threshold:
- Zoomed out: decade ticks + labels, half-decade minor ticks
- Zoomed in: every-year ticks, with decade/quinquennial ticks emphasized and labeled larger, other years get a small tick (and a label too, if `pxPerYear >= 35`)

## Proposed Approach

- Add one more threshold above the existing "every year" tier. Past it, draw four evenly-spaced minor ticks between each pair of consecutive year ticks.
- Keep them visually subordinate to year ticks — shorter/thinner, consistent with how the existing tiers already de-emphasize minor ticks relative to major ones.
- Decide whether quarters get any label at all (a small "Q1"/"Q2" on hover via tooltip, versus no label ever) — lean toward no persistent label, tooltip-only if anything, to avoid axis clutter at a tier this fine.

## Acceptance Criteria

- [ ] A new, clearly higher `pxPerYear` threshold (above the current every-year tier) reveals quarter hashmarks between year ticks
- [ ] Quarter ticks are visually subordinate to year ticks in the same style the existing tier hierarchy already establishes
- [ ] Reaching this tier requires deep zoom (via AE-3's input) on a normal single year or two, not the full timeline

## Non-Goals

- No month- or day-level ticks beyond quarters — not requested, don't build ahead of an actual need
- Doesn't decide whether individual events eventually anchor to quarter-precision dates — that's a data question, not an axis-rendering one

## Related

- Epic: [AE-1](AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2 (and practically, AE-3, to reach this zoom depth by hand)
