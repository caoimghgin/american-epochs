---
id: AE-6
title: Event density vs. zoom depth (curation)
type: Story
status: Done
priority: Low
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [dataviz, content, zoom]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Decide which events — and which of the existing tier 1/2/3 assignments — should appear at which zoom depth, once continuous zoom actually exists to observe it in practice. Explicitly deferred rather than designed alongside the navigation rebuild.

## Motivation

Directly from design discussion: "I haven't determined what events should show at what resolution yet... I didn't muck in all of the events yet." Tracked here so the question isn't lost, without blocking AE-2 through AE-5 on an answer that isn't ready.

## Current State

- `EVENTS` entries carry a fixed `tier` (1/2/3), currently filtered by the Overview/Standard/Detail buttons (`zoomLevel`).
- Unclear, post-rebuild, whether that three-button UI persists as-is, gets replaced by continuous depth-based reveal, or both coexist (buttons as a coarse override, depth as the default).
- Event density in the current dataset is uneven across the timeline — some 50-year windows have a couple dozen tier-1/2 events, others far fewer — so whatever rule gets chosen needs to hold up across genuinely different local densities, not just the busiest stretch.

## Proposed Approach

Resolved once AE-2 through AE-5 landed and a real, professionally-researched event corpus (AE-10's House Divided data) made the question concrete instead of hypothetical.

## Acceptance Criteria

- [x] A defined depth scheme exists, replacing the ad hoc tier 1/2/3
- [x] The Depth control supports however many levels the scheme calls for
- [x] Verified against a genuinely dense real stretch (not a hypothetical) that deeper levels reveal more without the shallower ones ever feeling incomplete

## Implementation Notes

Resolved by adopting the 5-level scheme that came with AE-10's House Divided event corpus rather than inventing one from scratch: **Spine** (persistent at every zoom, the epoch's narrative backbone) → **Major** (default) → **Mechanism** (explains shifts/consequences) → **Context** (political/social/institutional texture) → **Detail** (closest inspection). `tier` now ranges 1–5 instead of 1–3; the Depth control grew from 3 buttons to 5, generalized into a `depthBtns` array rather than hand-wiring each one.

This is a **mechanism + scheme** resolution, not full content coverage: only The House Divided (1854–1865) is actually populated at 5-level density so far (101 events researched, 100 kept after dropping one near-duplicate). Every other epoch's events still carry their original tier 1–3 values, which remain valid under the new scheme (they simply don't yet have anything at tier 4–5) — extending that same depth to other epochs is content work, tracked under AE-10, not a further design or mechanism question here.

Verified via Playwright at the actual House Divided density: Spine shows 18 events in view, Major (default) 31, Detail 86 — confirmed via direct DOM/screenshot inspection that labels decline gracefully under crowding rather than becoming illegible, and that zooming in (rather than a hypothetical smarter collision algorithm) is what resolves density, consistent with how every other layer in this instrument already handles crowding.

## Non-Goals

Extending the 5-level scheme's actual content to epochs beyond The House Divided — that's AE-10's scope, epoch by epoch.

## Related

- Epic: [AE-1](AE-1-continuous-zoom-rebuild.md)
- Practically follows: AE-2, AE-3, AE-4, AE-5
- Resolved alongside: [AE-10](../AE-10-event-date-backfill.md) (House Divided's event corpus, which supplied the actual 5-level scheme)
- Resolved alongside: [AE-10](../AE-10-event-date-backfill.md) (House Divided's event corpus, which supplied the actual 5-level scheme)
