---
id: AE-9
title: Event date precision — mechanical support
type: Story
status: Done
priority: Medium
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [content, dataviz, axis]
components: [src/american-epochs.html]
epic: AE-8
---

## Summary

Add optional `month`/`day` (and `endMonth`/`endDay` for spans) fields to `EVENTS`, a positioning helper that computes an accurate fractional-year position from them, and precision-aware date formatting for tooltips and the detail panel. No data backfill — that's [AE-10](../AE-10-event-date-backfill.md).

## Motivation

The lower-hanging half of [AE-8](../AE-8-event-date-precision.md): unlocks the ability to give any event a precise date the moment it's warranted, without waiting on the larger backfill pass across all ~150 events.

## Current State (before this story)

- `EVENTS` entries: `{ y: 1941, tier: 1, label: 'Lend-Lease', ... }` — year only.
- `xOf(year)` already accepted fractional years correctly (a plain linear formula) — confirmed no change needed there.
- `drawEvents`, `computeSpanLayout`, and `drawSpans` all read `ev.y`/`ev.end` directly for positioning.

## Acceptance Criteria

- [x] `EVENTS` entries support optional `month`/`day` (point events) and `endMonth`/`endDay` (span events, alongside the existing `month`/`day` for the start) — all additive, none required
- [x] A helper computes accurate fractional-year position from year+month+day, accounting for actual days-in-month (leap years included) — used only for positioning, never for epoch-membership filtering or sorting, which stay on the plain integer year
- [x] Tooltips and the detail panel display dates at whatever precision is actually known — "1941" / "March 1941" / "March 11, 1941" — never fabricating a day or month that isn't documented
- [x] Existing year-only events are visually and behaviorally unchanged (verified, not just assumed)

## Implementation Notes

- `daysInMonth(y, m)` — `new Date(y, m, 0).getDate()`, the standard trick (`m` 1-based; day 0 of month `m` is the last day of month `m-1` in JS's 0-based month indexing) for correct month lengths including leap years.
- `fractionalYear(y, month, day)` — positioning-only; returns the plain integer `y` unchanged when `month` is absent, so nothing about existing behavior changes for the ~147 events that stay year-only until AE-10.
- `formatEventDate(y, month, day)` — same precision-tiered logic for display, using a new `MONTH_NAMES` array (full names, for prose) alongside the existing `MONTH_ABBR` (3-letter, for the compact axis labels from AE-5).
- Span events needed a second pair of fields (`endMonth`/`endDay`) since a span has two ends that can each carry independent precision. The old `+0.4` "tiny nudge so the end-cap is visible" (calibrated for whole-year spans, ~146 days — wildly too coarse at day precision) now only applies when `endMonth` is absent, preserving exact prior rendering for every span event until it's actually given a precise end date.
- Backfilled three unambiguous, well-documented dates as a working demonstration (not the AE-10 backfill pass): Fort Sumter (Apr 12, 1861), Lend-Lease (Mar 11, 1941), Pearl Harbor (Dec 7, 1941), and World War II's span (Dec 7, 1941 – Sep 2, 1945, the US-entry-to-formal-surrender framing already implied by the existing 1941–1945 years).
- Verified via Playwright: Lend-Lease and Pearl Harbor — both `y:1941` — now render 469px apart at deep zoom instead of overlapping at January 1st; tooltips and detail panel show "March 11, 1941" / "December 7, 1941" / "December 7, 1941–September 2, 1945" correctly; a year-only event (Bretton Woods) still displays as plain "1944" with no regression — zero console errors.

## Non-Goals

- No data backfill beyond the three demonstration events — see AE-10
- No day-level axis tier — out of scope per AE-8's own Non-Goals

## Related

- Epic: [AE-8](../AE-8-event-date-precision.md)
- Sibling: [AE-10](../AE-10-event-date-backfill.md) (backfill pass)
