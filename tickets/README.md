# Tickets

Lightweight, JIRA-shaped tickets for work on this project, tracked as files instead of in an issue tracker. One ticket per file: `<KEY>-<title-slug>.md`, project key `AE`.

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
| [AE-1](AE-1-continuous-zoom-rebuild.md) | Continuous zoom rebuild — flexible-scale timeline navigation | Epic | In Progress | Low |
| [AE-2](AE-2-continuous-view-state-core.md) | Continuous view-state core | Story | Done | High |
| [AE-3](AE-3-wheel-zoom-drag-pan.md) | Wheel-zoom + drag-pan input | Story | Backlog | High |
| [AE-4](AE-4-density-floor-side-scroll.md) | Density floor + side-scroll for full-history view | Story | Backlog | Medium |
| [AE-5](AE-5-quarter-year-axis-tier.md) | Quarter-year hashmark axis tier | Story | Backlog | Low |
| [AE-6](AE-6-event-density-curation.md) | Event density vs. zoom depth (curation) | Story | Backlog | Low |
