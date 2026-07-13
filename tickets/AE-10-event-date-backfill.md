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

Grew beyond its original scope (just backfilling month/day onto existing events) into something bigger: replacing each epoch's placeholder-level event list with a professionally-researched, multi-level corpus — full date precision plus AE-6's 5-level depth scheme (Spine/Major/Mechanism/Context/Detail) — one epoch at a time. **A House Divided is done**; the other 13 epochs still have their original, much thinner tier 1-3 event sets.

## Motivation

AE-9 built the mechanism (schema + positioning + display). Kevin supplied a genuinely rich, well-sourced 101-event YAML corpus for A House Divided, organized into exactly the 5-level depth hierarchy AE-6 had been waiting to resolve — turning "backfill some dates" into "adopt real per-epoch research at real depth."

## Current State

- **A House Divided (1854–1865): done.** 101 YAML events reviewed, 100 kept (one dropped as a near-duplicate of the Kansas-Nebraska Act entry), reconciled against the ~10 events already there. 83 became point events across all 5 tiers; 17 genuinely long-running phenomena (2+ weeks) became new `span:true` Duration-strip entries — short 1–7 day battles/conventions/trials stayed as point events rather than cluttering the Duration strip with near-instantaneous "spans." One factual correction caught and fixed before merge: the 14th Amendment's ratification date (a Reconstruction-epoch entry, wrong by one month).
- **Spine trim (follow-up):** initial merge left 17 events at tier:1 (Spine), too crowded at the widest zoom. Trimmed to 10: Kansas-Nebraska Act, Dred Scott, John Brown's raid, Lincoln elected, South Carolina secedes, Fort Sumter, Emancipation Proclamation, Gettysburg, Appomattox, Lincoln assassinated (the last two promoted from tier:2; Lincoln's assassination gained `month:4, day:14` and a proper note in the process, having previously been a bare year-only entry with no note). The other 9 original Spine entries moved down rather than disappearing: Republican Party founded, Caning of Charles Sumner, Confederate States formed, First Battle of Bull Run, Gettysburg Address, Sherman captures Atlanta, and Lincoln reelected went to tier:2 (Major); Homestead Act and Morrill Land-Grant Act went to tier:3 (Mechanism), grouped with the epoch's other wartime legislation.
- **Spine trim, round 2, and epoch rename:** Kevin reframed what "Spine" means — not per-epoch relative importance, but the epoch's own tightest arc, one beat per structural hinge, meant to read as a "crown of the mountain" that gets progressively more detailed on zoom (this reframing also produced a design conversation about replacing the manual Depth buttons with automatic zoom-tied reveal, tracked separately — see Related). Trimmed Spine from 10 to 7: Kansas-Nebraska Act, Lincoln elected, Fort Sumter, Emancipation Proclamation, Gettysburg, Appomattox, Lincoln assassinated. Dred Scott, John Brown's raid, and South Carolina secedes moved down to tier:2 (Major). In the same pass, the epoch itself was renamed from "The House Divided" to "A House Divided" project-wide (`EPOCHS` entry, code comments, README, CLAUDE.md) — Lincoln's own 1858 phrase and speech title ("House Divided" speech) were left unchanged since they're a historical quotation, not the epoch's own title.
- Every other epoch's events are still in their original, much sparser tier 1–3 state from the original 1854–1941 and 1941–present expansion passes.
- A `reconstruction_events.yaml` draft already exists (19 events, older 0/2-level schema, not yet the richer 5-level format) — the natural next candidate.

## Proposed Approach

- One epoch at a time, not a single mechanical pass across everything.
- Each epoch needs its own well-researched corpus (like House Divided's) rather than a generic backfill — this is real historical curation work per epoch, not just date-lookup.
- Before merging any future epoch's data: verify factual claims that can be checked (dates especially), reconcile against whatever's already in `EVENTS` for that epoch, and make an editorial call on span vs. point treatment for multi-day events.
- Hold the same honesty bar as the rest of the project (register system, crime-lane coverage notes): no fabricated precision where the actual date isn't genuinely known.

## Acceptance Criteria

- [x] A House Divided has full 5-level depth and date precision, verified via Playwright at real density (18/31/86 events across Spine/Major/Detail at initial merge; Spine trimmed to 10, then to 7, in follow-up passes, each confirmed by direct count against the source)
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
- Prompted [AE-11](AE-11-zoom-driven-progressive-reveal.md) (replacing the manual Depth control with automatic zoom-tied reveal)
