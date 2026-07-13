---
id: AE-11
title: Zoom-driven progressive event reveal (replace manual Depth control)
type: Story
status: Backlog
priority: Medium
reporter: Kevin Muldoon
assignee: Unassigned
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

- [ ] Manual Depth buttons removed from the UI
- [ ] Tier visibility is driven entirely by `zoomScale`, calibrated against real data (not guessed thresholds)
- [ ] Event dots show a 5-step shade ramp by tier
- [ ] Verified that zooming from full history down into a single epoch reveals tiers progressively, with no jarring pop-in of a whole tier at once feeling arbitrary

## Non-Goals

- Doesn't itself re-curate what counts as tier 1–5 across every epoch — that's content work, tracked under AE-10 (and partially anticipated by A House Divided's two Spine trims)
- Doesn't resolve the open question of whether "Spine" should eventually mean a single global significance ranking across all 14 epochs, rather than per-epoch — flagged during the same conversation but not decided

## Related

- Follows from: [AE-10](AE-10-event-date-backfill.md)'s Spine-trim discussion for A House Divided
- Reuses: `zoomScale` metric and calibration workflow from [AE-5](completed/AE-5-quarter-year-axis-tier.md)
- Reopens part of: [AE-6](completed/AE-6-event-density-curation.md)'s resolution, which introduced the manual Depth control this ticket removes
