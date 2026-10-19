---
id: AE-15
title: Source-verify the remaining Gilded Age dates and factual claims
type: Task
status: Backlog
priority: Medium
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-10-17
labels: [content, research, verification]
components: [src/data/gilded_age_events.yaml, src/american-epochs.html]
epic: AE-8
---

## Summary

The Gilded Age corpus (and, as of 2026-10-17, the Big Stick corpus too) was written by Claude from memory, with no source URLs. Of its 118 events, 21 have a date confirmed against an identifiable source. The other 97, mostly Levels 3–5, still need checking, along with the numeric and attributive claims in their descriptions.

## Motivation

The project's honesty bar (register system, no fabricated precision) applies to the data as much as the prose. A first external review by ChatGPT and a round of Claude web searches settled the highest-visibility dates, but ChatGPT's own review said it had not completed a full audit, and its spreadsheet carries source URLs on only 17 of 118 rows. Left alone, this corpus is the one epoch whose dates rest on recall, and later epochs built the same way would compound the problem.

## Current State

- 21 of 118 events carry `date_verified: true` in `gilded_age_events.yaml`: the federal acts and Supreme Court decisions confirmed in `src/data/gilded_age_date_audit.csv`, the Great Railroad Strike start, the Federation of Organized Trades founding, Carnegie Steel's consolidation, and Munn, the Interstate Commerce Act, Mississippi 1890, Turner, the Standard Oil Trust, the silver-purchase repeal, Niagara, and Carlisle.
- Ambiguous cases were loosened rather than guessed: Pullman and Homestead end at month level (`end_precision: month`), Hull House is month-level, and the Readjuster election is year-level.
- `gilded_age_date_audit.csv` is a ChatGPT product. Its depression row is stale (it audited the pre-fix 1897-03-31 end date), and 100 of its rows read "not independently verified," so treat those as no information.
- Reconstruction's dates were spot-checked when its corpus was reconciled; A House Divided's came from a researched package. Neither has had a systematic audit.

## Proposed Approach

- Work through the 97 unverified events by level, highest first, adding `date_verified: true` once a date matches an authoritative source (National Archives, Library of Congress, Supreme Court reports, NPS, state archives, a university press).
- Check the claims that go beyond dates: numbers (death tolls, vote counts, membership, acreage), names, and attributions. Examples to check first: the Johnstown death toll, the Exodusters' migration figures, Native land totals under the Dawes Act, Weaver's electoral votes, and the Pullman and Homestead casualty counts.
- For any date that cannot be pinned down, lower `date_precision` instead of keeping a guess.
- Record a source URL on each verified entry (the YAML's `sources` key) so the audit trail survives. Reconciliation does not carry sources into the live array, so no change to `american-epochs.html` is needed unless a date moves.
- Anything corrected goes into both the YAML and the live `EVENTS` array, per `EVENT_CORPUS_GUIDE.md`'s reconciliation rules.
- Consider extending the same audit to Reconstruction and A House Divided afterward, since neither has had one.

## Acceptance Criteria

- [ ] Every Level 1 and Level 2 event has `date_verified: true` and at least one source
- [ ] Levels 3–5 are each either verified or carry a lowered `date_precision`
- [ ] Numeric and attributive claims in descriptions spot-checked, with corrections applied to the live array
- [ ] `gilded_age_events.yaml` and `EVENTS` agree after the pass (tier counts and dates), verified by script
- [ ] The corpus guide's status row and AE-10's Gilded Age note updated to say the epoch is source-verified

## Scope note

`big_stick_events.yaml` (114 events) and `new_freedom_events.yaml` (105 events), `return_to_normalcy_events.yaml` (104 events), and `new_deal_events.yaml` (105 events), and `american_ascendancy_events.yaml` (106 events), and `affluent_society_events.yaml` (117 events), `unravelling_events.yaml` (121 events), and `morning_in_america_events.yaml` (114 events) have the same status as the Gilded Age had before its audit: nothing is source-verified. Run the same procedure on it, preferably after the Gilded Age is finished so the method is settled. The title still says Gilded Age because that is where the ticket started.

## Non-Goals

- Doesn't re-argue Level assignments (those were settled with the outline)
- Doesn't change the thesis or the Level 1 spine
- Doesn't add new events beyond fixing anything the audit shows to be wrong or misdated

## Related

- Parent: [AE-10](AE-10-event-date-backfill.md) (epoch-by-epoch corpus adoption)
- Epic: [AE-8](AE-8-event-date-precision.md)
- Source of the first partial audit: `src/data/gilded_age_date_audit.csv`
