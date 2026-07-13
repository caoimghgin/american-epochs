---
id: AE-10
title: Epoch-by-epoch event corpus adoption (date precision + 5-level depth)
type: Story
status: In Progress
priority: Low
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-12
labels: [content, research]
components: [src/american-epochs.html]
epic: AE-8
---

## Summary

Grew beyond its original scope (just backfilling month/day onto existing events) into something bigger: replacing each epoch's placeholder-level event list with a professionally-researched, multi-level corpus — full date precision plus AE-6's 5-level depth scheme (Spine/Major/Mechanism/Context/Detail) — one epoch at a time. **The House Divided is done**; the other 13 epochs still have their original, much thinner tier 1-3 event sets.

## Motivation

AE-9 built the mechanism (schema + positioning + display). Kevin supplied a genuinely rich, well-sourced 101-event YAML corpus for The House Divided, organized into exactly the 5-level depth hierarchy AE-6 had been waiting to resolve — turning "backfill some dates" into "adopt real per-epoch research at real depth."

## Current State

- **The House Divided (1854–1865): done.** 101 YAML events reviewed, 100 kept (one dropped as a near-duplicate of the Kansas-Nebraska Act entry), reconciled against the ~10 events already there. 83 became point events across all 5 tiers; 17 genuinely long-running phenomena (2+ weeks) became new `span:true` Duration-strip entries — short 1–7 day battles/conventions/trials stayed as point events rather than cluttering the Duration strip with near-instantaneous "spans." One factual correction caught and fixed before merge: the 14th Amendment's ratification date (a Reconstruction-epoch entry, wrong by one month).
- Every other epoch's events are still in their original, much sparser tier 1–3 state from the original 1854–1941 and 1941–present expansion passes.
- A `reconstruction_events.yaml` draft already exists (19 events, older 0/2-level schema, not yet the richer 5-level format) — the natural next candidate.

## Proposed Approach

- One epoch at a time, not a single mechanical pass across everything.
- Each epoch needs its own well-researched corpus (like House Divided's) rather than a generic backfill — this is real historical curation work per epoch, not just date-lookup.
- Before merging any future epoch's data: verify factual claims that can be checked (dates especially), reconcile against whatever's already in `EVENTS` for that epoch, and make an editorial call on span vs. point treatment for multi-day events.
- Hold the same honesty bar as the rest of the project (register system, crime-lane coverage notes): no fabricated precision where the actual date isn't genuinely known.

## Acceptance Criteria

- [x] The House Divided has full 5-level depth and date precision, verified via Playwright at real density (18/31/86 events across Spine/Major/Detail)
- [ ] Reconstruction (1865–1877) gets the same treatment, starting from the existing draft YAML
- [ ] Remaining 12 epochs get the same treatment over time — no deadline, no required order

## Non-Goals

- Doesn't require reaching every epoch before this ticket can be considered valuable — each epoch's pass is independently useful, this is explicitly incremental
- Doesn't touch epoch start/end boundaries (still deliberately whole-year, per AE-8's Non-Goals)
- Doesn't fabricate precision — an epoch can stay partially year-only if that's honestly where the research lands

## Related

- Epic: [AE-8](AE-8-event-date-precision.md)
- Depends on: [AE-9](completed/AE-9-event-date-mechanical-support.md)
- Resolved [AE-6](completed/AE-6-event-density-curation.md) (the 5-level depth scheme came from this work)
