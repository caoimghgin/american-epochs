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
---
```

Body sections, in order:

- **Summary** — one or two sentences, what this is
- **Motivation** — why it's worth doing, what prompted it
- **Current State** — what exists today, so the ticket doesn't go stale as the codebase moves
- **Proposed Approach** — the shape of the fix/build, not a full spec
- **Acceptance Criteria** — checkboxes; a ticket is Done when these are checked
- **Non-Goals** — explicitly out of scope, so nobody re-litigates it mid-implementation
- **Related** — links to other tickets, commits, or external references

## Index

| ID | Title | Type | Status | Priority |
|---|---|---|---|---|
| [AE-1](AE-1-continuous-zoom-rebuild.md) | Continuous zoom rebuild — flexible-scale timeline navigation | Epic | Backlog | Low |
