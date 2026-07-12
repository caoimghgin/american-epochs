---
id: AE-5
title: Sub-year axis tiers (quarters + months) and a hard max-zoom cap
type: Story
status: Done
priority: Low
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [dataviz, rewrite, zoom, axis]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Add two new, deeper tiers to `drawAxis()` past the existing every-year tier — quarter-year hashes, then month ticks — and a hard maximum zoom level past which the view can't go tighter. Grew beyond the original "quarters only" scope once the calibration exercise revealed a natural third tier (months) and the need for an explicit ceiling, rather than the arbitrary fixed 2-year minimum span that existed before.

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

- [x] A new, clearly higher `pxPerYear` threshold (above the current every-year tier) reveals quarter hashmarks between year ticks
- [x] Quarter ticks are visually subordinate to year ticks in the same style the existing tier hierarchy already establishes
- [x] Reaching this tier requires deep zoom (via AE-3's input) on a normal single year or two, not the full timeline
- [x] A further tier past quarters shows month ticks, with quarter-aligned months slightly emphasized
- [x] The view has a hard, deliberately chosen maximum zoom — it doesn't drift arbitrarily tighter with continued wheel-in

## Implementation Notes

- **Calibrated live, not guessed.** Added a temporary floating readout (`zoom: N.NN`) driven by a "zoom scale" metric — `Math.log2(pxPerYear / FLOOR_PX_PER_YEAR)`, so 0 sits exactly at AE-4's floor density and each +1 is a doubling of density from there, similar in spirit to map-tile zoom levels. Kevin zoomed around with it on screen and reported back concrete numbers: quarters from **2.72**, months from **3.18**, hard cap at **4.85**. The readout and its HTML/CSS were removed once these were captured — see commit history for the temporary version if it's ever needed again.
- `QUARTER_ZOOM_SCALE`, `MONTH_ZOOM_SCALE`, and `MAX_ZOOM_SCALE` are stored as the calibrated scale values directly (not pre-converted to raw px/year), so the thresholds in code read the same way Kevin specified them. `drawAxis()` recomputes the same `zoomScale` expression locally to compare against them.
- Quarters render as 3 interior hashes per year (at +0.25/0.5/0.75 — 3 dividers make 4 equal segments, not 4 hashes as the original ticket phrasing suggested). Months render as 11 interior ticks (+1/12 .. +11/12), with every 3rd (quarter-aligned) month slightly taller/heavier, mirroring how decade/quinquennial ticks are already emphasized within the year tier.
- **Follow-up after first ship**: the initial pass used `--ink-faint` throughout and no labels, which read as too subtle once actually seen live. Bumped both tiers' size/weight and moved quarter-aligned marks (quarter tier itself, and every 3rd month in the month tier) to `--ink-soft` so they read as a real tier, not an afterthought — plain months stay `--ink-faint`, still the finest mark on the axis. Added abbreviated month-name labels (`Jan`, `Feb`, ...) below each month tick, but only once `pxPerYear/12 > 22px` — the same "does it fit" instinct used for epoch bands, the cycle rail, and event labels — so labels never crowd at the low end of the month tier, only once there's genuinely room.
- **The max-zoom cap replaces AE-2's arbitrary fixed `MIN_VIEW_SPAN = 2` (years)** with a computed `minViewSpan`, derived from `MAX_PX_PER_YEAR` and the current container width the same way AE-4's `maxViewSpan` is derived from `FLOOR_PX_PER_YEAR` — both bounds are now expressed as a density (px/year) rather than one being density-based and the other a flat year-count, which is more consistent and (unlike a fixed year-span) scales correctly with the window's actual width.
- Verified via Playwright without relying on the (now-removed) debug readout: empirically measured `pxPerYear` from the pixel spacing between two adjacent labeled year ticks, converted to the same zoom-scale formula, and confirmed the unlabeled-tick count jumps at the right points (10 → 44 crossing 2.72; 44 → 118 crossing 3.18) and that `pxPerYear` freezes at exactly 634.5 (the value implied by scale 4.85) even after 20 further zoom-in wheel events — zero console errors.

## Non-Goals

- No day-level ticks beyond months — not requested, don't build ahead of an actual need
- Doesn't decide whether individual events eventually anchor to quarter- or month-precision dates — that's a data question, not an axis-rendering one

## Related

- Epic: [AE-1](../AE-1-continuous-zoom-rebuild.md)
- Depends on: AE-2 (and practically, AE-3, to reach this zoom depth by hand)
