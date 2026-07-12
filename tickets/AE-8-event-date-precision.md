---
id: AE-8
title: Event dates support year-month-day precision
type: Epic
status: Backlog
priority: Medium
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-12
labels: [content, dataviz, axis]
components: [src/american-epochs.html]
---

## Summary

Extend `EVENTS` entries with optional `month` (1–12) and `day` (1–31) fields — `y` stays the required baseline — so an event can render at its true position once zoomed deep enough (AE-5's quarter/month axis tiers, and full year-month-day precision at max zoom) instead of every event in a year snapping to January 1st.

## Motivation

Raised directly after AE-5 shipped: the axis now resolves down to individual months, and at max zoom (~634px/year) there's roughly 1.7px per day — enough pixel budget to meaningfully separate two events in the same month, not just the same year. `EVENTS` currently only carries a year, so Lend-Lease (documented as March 1941 in its own rationale text) renders identically to an event that actually happened in December 1941.

## Current State

- `EVENTS` entries: `{ y: 1941, tier: 1, label: 'Lend-Lease', note: '...' }` — year only, no month/day field exists.
- `xOf(year)` already accepts fractional years correctly (it's a plain linear formula) — no change needed there once an event has a fractional effective position.
- Many events' `note` text already states the actual month (occasionally the day) in prose — e.g. "Lend-Lease (March 1941)" — so backfilling is partly data-extraction from text already written, not all fresh research.
- `drawEvents`, `computeSpanLayout`, and `epochDetailHTML` all read `ev.y` directly today for both positioning and year-based filtering (epoch membership, sorting, lane assignment).

## Proposed Approach

- Add optional `month` and `day` fields to `EVENTS` entries — both additive, absent by default, never required. An event with just `y` behaves exactly as today; `y`+`month` positions within the year; `y`+`month`+`day` positions within the month, accounting for actual days-in-month (leap years included).
- A small helper computes an effective fractional year *for positioning only* (`eventFractionalYear(ev)`); `ev.y` itself stays the plain integer everything else already relies on (epoch-membership checks, tier filtering, sorting don't need to change).
- Detail panel / tooltip date display becomes precision-aware: "1941" / "March 1941" / "March 11, 1941," depending on what's actually known — never fabricating a day that isn't documented.
- Two distinct chunks of work, likely worth splitting into stories once picked up (same pattern as AE-1 and AE-7):
  1. **Mechanical support** — schema fields, the positioning helper, precision-aware date formatting. Small, contained, no data entry required. This is the lower-hanging half.
  2. **Backfill pass** across the ~150 existing events — real content work: extracting months/days already sitting in existing prose where available, leaving events at year-only precision where a month/day genuinely isn't part of the existing research (honest gaps, not filled in with guesses).

## Acceptance Criteria

Not yet defined — break into stories (mechanical support vs. backfill) once picked up.

## Non-Goals

- Doesn't add a dedicated "day" tier to the axis itself (individual day tick marks/labels) — that's a separate question from data precision; AE-5's month tier plus precision-aware tooltips may already be enough, revisit only if it's actually wanted
- Doesn't touch epoch start/end boundaries — those are deliberately whole-year, interpretive choices already justified in each epoch's rationale prose, not point-in-time events

## Related

- Follows directly from [AE-5](completed/AE-5-quarter-year-axis-tier.md) (sub-year axis tiers) — filed once that shipped and the data-precision gap became visible
- Precedent for splitting research/backfill from mechanical work: [AE-7](AE-7-extend-epochs-to-1775.md)
