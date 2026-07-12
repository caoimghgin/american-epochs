---
id: AE-6
title: Event density vs. zoom depth (curation)
type: Story
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-07-12
labels: [dataviz, content, zoom]
components: [src/american-epochs.html]
epic: AE-1
---

## Summary

Decide which events — and which of the existing tier 1/2/3 assignments — should appear at which zoom depth, once continuous zoom actually exists to observe it in practice. Explicitly deferred rather than designed alongside the navigation rebuild.

## Motivation

Directly from design discussion: "I haven't determined what events should show at what resolution yet... I didn't muck in all of the events yet." Tracked here so the question isn't lost, without blocking AE-2 through AE-5 on an answer that isn't ready.

## Current State

- `EVENTS` entries carry a fixed `tier` (1/2/3), currently filtered by the Overview/Standard/Detail buttons (`zoomLevel`).
- Unclear, post-rebuild, whether that three-button UI persists as-is, gets replaced by continuous depth-based reveal, or both coexist (buttons as a coarse override, depth as the default).
- Event density in the current dataset is uneven across the timeline — some 50-year windows have a couple dozen tier-1/2 events, others far fewer — so whatever rule gets chosen needs to hold up across genuinely different local densities, not just the busiest stretch.

## Proposed Approach

Not yet designed. Revisit once AE-2 through AE-5 have landed and it's possible to actually pan/zoom continuously and watch real crowding happen, rather than guessing at thresholds against a static mockup.

## Acceptance Criteria

TBD — this ticket exists to hold the open question, not to spec the answer prematurely.

## Non-Goals

N/A yet.

## Related

- Epic: [AE-1](AE-1-continuous-zoom-rebuild.md)
- Practically follows: AE-2, AE-3, AE-4, AE-5
