# Tickets

Lightweight, JIRA-shaped tickets for work on this project, tracked as files instead of in an issue tracker. One ticket per file: `<KEY>-<title-slug>.md`, project key `AE`.

Tickets move to `completed/` once their status is set to `Done` — `git mv`, not copy, so history follows the file. Everything still in `tickets/` (not `completed/`) is active or backlog. Update any cross-references (`[AE-N](...)`) when moving a file — links from `tickets/*.md` into `completed/` need a `completed/` prefix; links from `completed/*.md` back out need `../`.

## Format

Each ticket is a YAML frontmatter block followed by a fixed set of sections.

```yaml
---
id: AE-1
title: Short summary of the work
type: Epic | Story | Task | Bug
status: Backlog | To Do | In Progress | Blocked | Done
priority: Low | Medium | High | Critical
reporter: who filed it
assignee: who's doing it (Unassigned if nobody yet)
created: YYYY-MM-DD
labels: [free-form, tags]
components: [affected files or areas]
epic: AE-N   # only on Stories/Tasks that belong to an Epic; omit otherwise
---
```

Body sections, in order:

- **Summary** — one or two sentences, what this is
- **Motivation** — why it's worth doing, what prompted it
- **Current State** — what exists today, so the ticket doesn't go stale as the codebase moves
- **Proposed Approach** — the shape of the fix/build, not a full spec
- **Acceptance Criteria** — checkboxes; a ticket is Done when these are checked
- **Implementation Notes** — optional, added when a ticket is completed: what actually shipped, any decisions made mid-implementation that the original ticket left open, what got fixed along the way
- **Non-Goals** — explicitly out of scope, so nobody re-litigates it mid-implementation
- **Related** — links to other tickets, commits, or external references

## Index

| ID | Title | Type | Status | Priority |
|---|---|---|---|---|
| [AE-1](completed/AE-1-continuous-zoom-rebuild.md) | Continuous zoom rebuild — flexible-scale timeline navigation | Epic | Done | Low |
| [AE-2](completed/AE-2-continuous-view-state-core.md) | Continuous view-state core | Story | Done | High |
| [AE-3](completed/AE-3-wheel-zoom-drag-pan.md) | Wheel-zoom + drag-pan input | Story | Done | High |
| [AE-4](completed/AE-4-density-floor-side-scroll.md) | Density floor + side-scroll for full-history view | Story | Done | Medium |
| [AE-5](completed/AE-5-quarter-year-axis-tier.md) | Sub-year axis tiers (quarters + months) and a hard max-zoom cap | Story | Done | Low |
| [AE-6](completed/AE-6-event-density-curation.md) | Event density vs. zoom depth (curation) | Story | Done | Low |
| [AE-7](AE-7-extend-epochs-to-1775.md) | Extend epochs back to 1775 (the Revolution through Manifest Destiny) | Epic | Backlog | Low |
| [AE-8](AE-8-event-date-precision.md) | Event dates support year-month-day precision | Epic | In Progress | Medium |
| [AE-9](completed/AE-9-event-date-mechanical-support.md) | Event date precision — mechanical support | Story | Done | Medium |
| [AE-10](AE-10-event-date-backfill.md) | Epoch-by-epoch event corpus adoption (date precision + 5-level depth) | Story | In Progress | Low |
