---
id: AE-10
title: Backfill month/day precision across existing events
type: Story
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-12
labels: [content, research]
components: [src/american-epochs.html]
epic: AE-8
---

## Summary

Go through the ~150 existing `EVENTS` entries and add `month`/`day` (or `endMonth`/`endDay` for spans) wherever it's genuinely known, using the schema and rendering support [AE-9](completed/AE-9-event-date-mechanical-support.md) already shipped. Three obvious dates (Fort Sumter, Lend-Lease, Pearl Harbor, and the World War II span) are already done as AE-9's demonstration — this is the rest.

## Motivation

AE-9 built the mechanism; this is the actual content work that makes it pay off everywhere, not just on three cherry-picked events.

## Current State

- ~147 of ~150 `EVENTS` entries are still year-only.
- A meaningful fraction already state their actual month (sometimes day) directly in their own `note` prose — e.g. "Volcker's appointment (August 1979)," "Carmichael's 'Black Power' (June)" — so a real chunk of this is extracting a date that's already been researched and written down, not fresh research.
- The rest will need actual lookup — this is where the real time goes.

## Proposed Approach

- Not a single mechanical pass — go epoch by epoch (or tier by tier, starting with tier-1 structural pivots, since those matter most at deep zoom) rather than all 150 at once.
- Where the month/day is already in the `note` text, extract it directly.
- Where it isn't, look it up — but hold the same honesty bar the rest of the project already keeps (register system, the crime-lane coverage note, etc.): if a genuinely precise day isn't establishable or isn't really the point (e.g., a slow-moving trend rather than a discrete event), leave it at year-only rather than inventing a plausible-sounding date.
- Span events' *end* dates are often fuzzier than their starts (wars and crises rarely have as clean an end date as a start date) — expect more of these to stay year-only or month-only on the end side even once the start is precise.

## Acceptance Criteria

- [ ] Tier-1 structural-pivot events have month precision wherever it's genuinely documented
- [ ] Span events have start precision at least as often as tier-1 point events; end precision where it's real, left year-only where the actual end is genuinely fuzzy
- [ ] No fabricated days — every added `day` traces to an actual documented date, not an estimate
- [ ] Spot-checked at deep zoom: a densely-dated month (e.g. late 1979, several events close together) reads as legibly separated rather than still overlapping

## Non-Goals

- Doesn't require reaching 100% coverage — partial, honest backfill (some events precise, some still year-only) is the expected end state, not a bug to fix later
- Doesn't touch epoch start/end boundaries (still deliberately whole-year, per AE-8's Non-Goals)

## Related

- Epic: [AE-8](AE-8-event-date-precision.md)
- Depends on: [AE-9](completed/AE-9-event-date-mechanical-support.md)
