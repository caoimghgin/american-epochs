---
id: AE-13
title: Consolidate foundational editorial principles into a single canonical document
type: Task
status: Done
priority: Medium
reporter: Kevin Muldoon
assignee: Claude
created: 2026-07-19
labels: [docs, editorial-process]
components: [src/data/FOUNDATIONAL_EDITORIAL_PRINCIPLES.md, src/data/EVENT_CORPUS_GUIDE.md]
---

## Summary

Rewrite `src/data/FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` in place to absorb a
richer statement of the project's editorial philosophy — the "Three Layers"
framework (Historical Thesis / Foundational Question / Historical Answer),
history as an ongoing constitutional conversation, and a formalized
production pipeline — so there's one canonical document answering "how do we
build an American Epoch?" rather than that reasoning being scattered across
chat history.

## Motivation

Filed originally as a raw draft (this file, unfilled frontmatter, no
sections) containing a "Constitutional Conversation of America" essay Kevin
felt was important enough to become a standing design document, not just
implementation notes. Independently, a parallel conversation with ChatGPT
proposed a four-question/cluster-first production workflow (outline the
story before touching YAML; brainstorm 250–400 candidate events unfiltered;
cluster them conceptually, not chronologically; only then choose the Level 1
spine) plus an "epoch identity" paragraph and a per-epoch "handoff" sentence
bridging one epoch's answer to the next epoch's question. Both threads
converged on the same conclusion: the existing `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md`
already occupies the "read this first" slot (`EVENT_CORPUS_GUIDE.md` points to
it by name), so the new material should expand that file in place rather
than spawn a second doc competing for the same authority.

## Current State

Before this ticket, `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` covered: the two
jobs of the hierarchy, historical thesis + foundational question (two
layers, not three), event-selection tests, level logic, the recurring
American question, and the editorial goal. It did not cover a third
"Historical Answer" layer, the epoch-identity paragraph, the handoff
concept, the conversation-chain framing, or the cluster-before-spine
production sequence.

## Proposed Approach

- Rewrite `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` keeping its filename (no
  cross-reference churn in `CLAUDE.md`/`EVENT_CORPUS_GUIDE.md`), consolidating:
  Purpose, Epoch Identity, the Three Layers, the Handoff (concept only — the
  actual data-model/UI feature is out of scope here, see AE-14), History as
  an Ongoing Conversation, the Editorial Test, Progressive Disclosure, and a
  new "Editorial Review" section spelling out the outline → unfiltered
  candidate list → conceptual clustering → Level 1 selection → handoff draft
  → YAML sequence — named "Editorial Review" rather than "editor-in-chief"
  deliberately, since the process should survive any specific contributor.
- Keep `EVENT_CORPUS_GUIDE.md` as the separate mechanical format/schema spec
  rather than folding the two together; add a light pointer from its
  "Optional: a per-epoch outline" section back to the new Editorial Review
  sequence, and note that `reconstruction_outline.md` predates the
  epoch-identity/handoff additions.
- Do not backfill `reconstruction_outline.md` or any other existing outline
  with the new sections — that's real authoring work, not doc consolidation.

## Acceptance Criteria

- [x] `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` contains the Three Layers,
      Epoch Identity, Handoff concept, and conversation-chain framing
- [x] A formalized "Editorial Review" production sequence is documented,
      matching how Reconstruction was actually built (outline → cluster →
      spine → YAML)
- [x] `EVENT_CORPUS_GUIDE.md` cross-references the new process without
      duplicating it
- [x] No second competing philosophy document created
- [x] This ticket itself reformatted into the project's standard ticket
      shape and moved to `completed/`

## Implementation Notes

Resolved three open questions before writing: keep the existing filename
(vs. renaming to reflect wider scope) to avoid churning existing
cross-references; keep `EVENT_CORPUS_GUIDE.md` separate from the philosophy
doc rather than merging the two; and split off the `handoff` field's actual
data-model/UI implementation and 14-epoch backfill into its own ticket
(AE-14) rather than doing it inline with a documentation pass.

## Non-Goals

- Does not implement the `handoff` field in the live instrument's data model
  or UI — tracked as [AE-14](../AE-14-epoch-handoff-field.md)
- Does not backfill epoch-identity or handoff text into
  `reconstruction_outline.md` or any other existing per-epoch outline

## Related

- Produces the process now expected of: [AE-10](../AE-10-event-date-backfill.md)'s
  remaining epoch-by-epoch corpus work
- Split off: [AE-14](../AE-14-epoch-handoff-field.md), the handoff field's
  actual implementation
- Worked example the new Editorial Review sequence is calibrated against:
  `src/data/reconstruction_outline.md`
