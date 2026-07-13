---
id: AE-7
title: Extend epochs back to 1775 (the Revolution through Manifest Destiny)
type: Epic
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-12
labels: [content, epochs, research]
components: [src/american-epochs.html]
---

## Summary

Extend the epoch/cycle data back from 1854 (The House Divided's current start) to 1775 (Lexington and Concord — "the first shots" of the Revolutionary War) — roughly 6–8 more epochs, comparable in scope to the 1854–1941 epoch-expansion pass that produced The House Divided through A New Deal.

## Motivation

Raised directly in conversation: pushing the framework back to the start of the Revolutionary War. Comparably sized to the epoch-expansion work already shipped (1775–1854 is 79 years, versus the 87 years covered from 1854–1941 across 7 epochs) — but structurally harder, since it crosses three different party systems rather than extending one continuous framework the way 1854–1941 did. Referenced twice already as "a future ticket, not yet filed" (in AE-1's and AE-4's Non-Goals) without ever actually being filed — this closes that gap.

## Current State

- `EPOCHS` currently starts at The House Divided (1854), `cycle:null`, `phase:'Rupture'` — the same "rupture between two orders" treatment this new work would need to apply again, further back, at 1775.
- No epoch/cycle data exists before 1854. `PRESIDENTS` starts at Pierce (1853); `RECESSIONS` uses NBER data from December 1854 (NBER's own earliest dated contraction).
- `FLOOR_PX_PER_YEAR` (AE-4) was chosen against the narrowest *currently existing* epoch's label ("The New Freedom," 6 years) — not verified against whatever the narrowest epoch in this new span turns out to be.
- A rough sketch was discussed at length but never finalized or written into the codebase — see Proposed Approach.

## Proposed Approach

Rough sketch only, not a spec — needs its own research/naming pass with the same depth as the 1854–1941 work before anything lands in `EPOCHS`:

- **The Revolution (1775–1783)** — a second standalone Rupture, same treatment as The House Divided: belongs to no cycle, since it's the founding rupture the whole framework's constitutional order depends on, not a crisis phase of an existing one.
- Everything after likely forms at least one more full cycle:
  - *Genesis*: The Confederation (1783–1789) — the Articles of Confederation failing, Shays' Rebellion, the Constitutional Convention
  - *Rise*: The Federalist Republic (1789–1801ish) — Washington/Adams, the First Party System forming in real time
  - *Apex*: the Jeffersonian era (1801–1815ish) — Louisiana Purchase, the War of 1812 ("Ascendancy" is already taken by 1941–52, needs a different name)
  - *Crisis*, possibly double (matching the existing Crisis-pair pattern used twice already): Era of Good Feelings (1815–1828ish, deceptive one-party unity cracking on the Missouri Compromise) → Jacksonian Democracy / Manifest Destiny (1828–1854ish), feeding directly into The House Divided's 1854 start
- **Open methodological question, flagged but not resolved during the original discussion**: this span crosses three genuinely different party systems (First Party System, a one-party interregnum, Second Party System), each with different structural logic — property-qualified electorates vs. mass suffrage, no organized national parties before the 1790s, etc. The Rise→Apex→Crisis framework may need to flex, or split differently, in ways it hasn't had to anywhere else in the timeline so far. This is the real risk in the ticket, not the history itself.

## Acceptance Criteria

Not yet defined — this is an Epic to break into stories once picked up, the same way AE-1 was. At minimum: a research/naming pass matching the depth of the 1854–1941 work (vibe, contested naming, actual justification for each boundary year) has to happen before any epoch lands in the data.

## Non-Goals

- Not committing to the exact epoch names/boundaries above — that sketch is a starting point for discussion, not a spec
- Doesn't touch the continuous-zoom rebuild (AE-1 through AE-6) — this is a content/data addition, independent of that work, though it will require revisiting AE-4's `FLOOR_PX_PER_YEAR` against whatever the narrowest new epoch turns out to be

## Related

- Referenced as pending in [AE-1](completed/AE-1-continuous-zoom-rebuild.md) and [AE-4](completed/AE-4-density-floor-side-scroll.md)'s Non-Goals sections
- Precedent for scope/depth of work required: the 1854–1941 epoch-expansion pass, commit `4f099f8`
