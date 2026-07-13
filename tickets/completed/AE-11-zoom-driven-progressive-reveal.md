---
id: AE-11
title: Zoom-driven progressive event reveal (replace manual Depth control)
type: Story
status: Done
priority: Medium
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-13
labels: [dataviz, ux, zoom]
components: [src/american-epochs.html]
---

## Summary

Replace the manual Depth buttons (Spine/Major/Mechanism/Context/Detail) with automatic, continuous-zoom-driven reveal: tier 1 (Spine) always visible, tiers 2–5 unlocking progressively as the user zooms in. No user toggle — depth becomes a function of zoom level, not a choice the viewer makes for themselves.

## Motivation

Raised directly in conversation, using a mountain metaphor: "the crown of the mountain is the tippy most and then we progressively give more volume of information as we zoom in." Explicitly not a per-viewer preference: "I don't think I want the user to have control over what they see — we are going to be telling the story... We are the editors. We are the curators and we should take that job seriously." This reframes `tier` from a manual filter into an authored pacing device.

## Current State

- `zoomLevel` (1–5) is set only by clicking one of five Depth buttons (`depthBtns`, wired in the script near line 1394); `drawEvents()` filters with `ev.tier <= zoomLevel`.
- A continuous zoom-scale metric already exists and is unused for this purpose: `zoomScale = Math.log2(pxPerYear / FLOOR_PX_PER_YEAR)`, currently only driving axis-tick granularity (quarter/month thresholds from AE-5), ranging 0 to `MAX_ZOOM_SCALE` (4.85).
- Event dots currently use a binary shade: `ev.tier===1 ? 'var(--ink)' : 'var(--ink-soft)'` — no gradation across tiers 2–5.
- Tier assignment today is per-epoch (each epoch has its own Spine/Major/etc.), not a single global significance ranking — this ticket is the mechanism only; it doesn't require changing what "tier" means at the data level, though the two changes are related (see AE-10's note on A House Divided's Spine trim, which started reasoning about tier as a global "crown" rather than a per-epoch one).

## Proposed Approach

- Remove the five Depth buttons from the controls bar entirely.
- Derive an effective max-visible-tier directly from `zoomScale` on every frame, replacing the `zoomLevel` state variable. Reuse the floating scale-number readout (used previously to calibrate AE-5's axis thresholds) to tune the tier-reveal breakpoints live before committing to fixed values.
- Give event dots a 5-step shade ramp instead of the current binary one — tier 1 darkest (`--ink`), tier 5 lightest (near `--ink-faint`) — so depth reads visually even before a label is legible.
- Since the currently-visible epoch is already what's in the viewport, no separate "budget" logic is needed — zooming into one cycle or epoch naturally narrows the candidate pool without extra bookkeeping.

## Acceptance Criteria

- [x] Manual Depth buttons removed from the UI
- [x] Tier visibility is driven entirely by `zoomScale`, calibrated against real data (not guessed thresholds)
- [x] Event dots show a 5-step shade ramp by tier
- [x] Verified that zooming from full history down into a single epoch reveals tiers progressively, with no jarring pop-in of a whole tier at once feeling arbitrary

## Implementation Notes

- `zoomLevel` (manual state) and `depthBtns`/`setZoom` (button wiring) removed entirely; the Depth control group deleted from the controls bar.
- Added `TIER_REVEAL_SCALE = { 2: 0.9, 3: 1.8, 4: QUARTER_ZOOM_SCALE, 5: MONTH_ZOOM_SCALE }` and `maxVisibleTier(zoomScale)`, computed fresh in `drawEvents()` from the same `pxPerYear`/`zoomScale` calculation AE-5 already used for axis ticks. Tiers 4 and 5 deliberately reuse the quarter/month axis thresholds (2.72 / 3.18) rather than new guessed values — by the time there's screen room for finer date units, there's room for finer events too.
- Added `TIER_DOT_SHADE`, a 5-step ink ramp (`--ink` → `#3C3E3A` → `--ink-soft` → `#72756F` → `--ink-faint`) replacing the old binary tier-1-vs-everything-else fill.
- Verified via Playwright (a throwaway venv, since no system Playwright install was available this session): full-history default view showed 13 dots project-wide, all tier-1 shade; panning to A House Divided's own window at the same widest-zoom density showed exactly its 7 Spine events (plus each neighboring epoch's own tier-1 events) — confirmed by direct label list, not just a count. Zooming into a 2-year window produced all 5 shades simultaneously, confirming both the reveal thresholds and the shade ramp work together. No console errors at any zoom level tested.
- Discovered along the way (pre-existing, not caused by this change): the page's default scroll position inside the density-floor viewport opens near the present day (~1958–2027), not at 1852 — so A House Divided's own crown isn't visible without scrolling left first. Not fixed here; flagged in case it's worth its own ticket later.

## Non-Goals

- Doesn't itself re-curate what counts as tier 1–5 across every epoch — that's content work, tracked under AE-10 (and partially anticipated by A House Divided's two Spine trims)
- Doesn't resolve the open question of whether "Spine" should eventually mean a single global significance ranking across all 14 epochs, rather than per-epoch — flagged during the same conversation but not decided

## Related

- Follows from: [AE-10](../AE-10-event-date-backfill.md)'s Spine-trim discussion for A House Divided
- Reuses: `zoomScale` metric and calibration workflow from [AE-5](AE-5-quarter-year-axis-tier.md)
- Reopens part of: [AE-6](AE-6-event-density-curation.md)'s resolution, which introduced the manual Depth control this ticket removes
