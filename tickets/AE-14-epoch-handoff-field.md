---
id: AE-14
title: Epoch handoff field — data model, rendering, and 14-epoch backfill
type: Story
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-19
labels: [dataviz, content, editorial-process]
components: [src/american-epochs.html]
---

## Summary

Add a `handoff` field to each epoch — one or two sentences bridging that
epoch's Historical Answer to the next epoch's Foundational Question (see
"The Handoff" in `src/data/FOUNDATIONAL_EDITORIAL_PRINCIPLES.md`) — and
render it at the transition between epoch bands, then write the actual
handoff text for all 14 existing epochs.

## Motivation

Raised while consolidating the project's editorial philosophy (AE-13): the
handoff sentence was originally conceived as outline-only prose (something a
contributor writes in `<epoch_id>_outline.md` to check their own reasoning),
but it's strong enough to be part of the product itself, not just an
authoring aid — it's the concrete, reader-facing expression of the
"History as an Ongoing Conversation" chain the whole framework is built on.
Reconstruction's own handoff ("...the country's next great question would no
longer be who possessed rights — but who possessed power") is the reference
example.

## Current State

- `EPOCHS` entries (`id`, `name`, `start`, `end`, `color`, `cycle`, `phase`,
  `oneliner`, `rationale[]`) have no field for this; `oneliner` is a
  description of the epoch itself, not a bridge to its successor.
- No per-epoch outline currently has a drafted handoff sentence —
  `reconstruction_outline.md` predates the concept entirely.
- Epoch-band transitions in `drawEpochs()` currently render only the seam
  gradient between adjacent epochs' colors — no text content at the
  boundary.

## Proposed Approach

Rough sketch, not a spec:

- Add `handoff` (string) to each `EPOCHS` entry — the outgoing epoch's
  bridge into the next.
- Decide where it surfaces: candidates are the detail panel (shown when an
  epoch region is clicked, appended after its existing rationale) or a small
  inline marker at the epoch-to-epoch seam itself. The seam placement is
  more faithful to "the transition feels intentional" but has less room and
  more overlap risk with the existing gradient rendering.
- Author the actual handoff text for all 14 epochs, most straightforwardly
  as part of each epoch's own Editorial Review outline pass (AE-10) rather
  than as a single separate writing pass — but Reconstruction and A House
  Divided are already reconciled, so their two handoffs need writing
  independent of any pending corpus work.

## Acceptance Criteria

- [ ] `handoff` field added to the `EPOCHS` data model
- [ ] Rendered somewhere a viewer actually encounters it while navigating
      (not buried in a layer nobody toggles on)
- [ ] All 14 epochs have real handoff text (the standalone rupture, A House
      Divided, has no predecessor epoch to hand off *from*, but still hands
      off *into* Reconstruction)
- [ ] Verified in-browser that the transition reads as intentional, not as
      a duplicate summary of the epoch that just ended

## Non-Goals

- Doesn't change the existing `oneliner`/`rationale` fields or how they render
- Doesn't require re-deriving handoffs for epochs whose corpora aren't built
  yet (AE-10) before this field can ship — draft reasonable handoff text
  from the framework document even where the full event corpus is pending

## Related

- Concept and worked example defined in: `src/data/FOUNDATIONAL_EDITORIAL_PRINCIPLES.md`
  ("The Handoff")
- Split off from: [AE-13](completed/AE-13-foundational-editorial-principles-consolidation.md)
- Overlaps future work under: [AE-10](AE-10-event-date-backfill.md) (each
  remaining epoch's outline should draft its own handoff as it's authored)
