---
id: AE-16
title: Cross-epoch threads — surface themes that run across several epochs
type: Story
status: Backlog
priority: Low
reporter: Kevin Muldoon
assignee: Unassigned
created: 2026-10-17
labels: [dataviz, content, design]
components: [src/american-epochs.html, src/data/]
---

## Summary

Let a viewer follow one theme (immigration, the Great Migration, civil liberties, the income tax, antitrust) across the epochs it runs through, instead of seeing each event only inside its own epoch.

## Motivation

Raised during the Return to Normalcy outline, when the Immigration Act of 1924 turned out to be one beat in a thread that runs through the Chinese Exclusion Act (1882, Gilded Age), the Gentlemen's Agreement (1907, Big Stick), the 1921 and 1924 quota laws, and the 1965 abolition of national origins. The same pattern appears for the Great Migration (1916 onward, crossing five epochs), the income tax (Pollock 1895, the Sixteenth Amendment 1913, the War Revenue Act 1917), antitrust (Sherman 1890, Northern Securities 1904, Clayton 1914), and civil liberties (Espionage Act 1917 to Schenck, Debs, and Abrams). The project's central claim is that each epoch inherits an unresolved question from the last, and threads would make that visible at the level of single events.

## Update (2026-10-19): threads, not only themes

The editor's idea is larger than the tag filter above. A thread is a named, ordered group of events with a claim
about why they belong together. It may be a documented chain (Al-Qaeda's War) or an argued, looser grouping (The
Disenchantment, The Imperial Presidency), and it crosses epochs and levels. A pilot of three threads is drafted in
`src/data/threads_pilot.yaml`. What it showed about the data model:

- A thread has a title, a one-sentence claim, a short summary, a register (factual, structural, argued), and a
  counter-reading.
- Each member carries a role (origin, trigger, link, culmination, consequence, counterweight) and a link strength
  (documented, contested, argued), which is separate from the thread's register.
- Counterweights are members too, so that a thread reads as a contest and not a one-way story.
- One event can belong to several threads.
- Threads need stable event ids in the live array, which reconciliation currently drops.
- Threads surface gaps: the pilot proposes about twenty events that no corpus has yet, each needing a source.
- Threads are authored by Claude and reviewed by the editor, as Level 1 is. The interface is not designed yet.

## Current State

- Each event in the YAML corpora has a `tags` list (for example `immigration`, `income-tax`, `antitrust`, `civil-liberties`), so threads are already queryable in the research files.
- The live `EVENTS` array does not carry tags. Reconciliation drops them, per `EVENT_CORPUS_GUIDE.md`, so the instrument cannot yet filter or highlight by theme.
- Tags are not yet standardized across epochs (some say `speech`, some `free-speech`; some `migration`, some `great-migration`).

## Proposed Approach

Rough sketch, not a spec:

- Decide a small controlled vocabulary of thread names (maybe 8–12) and map the existing free-form tags onto it.
- Carry a `thread` field (or a short list) through reconciliation into the live array.
- In the UI, pick one of: a thread selector that highlights its events across the full timeline and dims the rest, a "related events" list in the detail panel, or a thin connector drawn between a selected event and its thread-mates. The selector is the cheapest and probably the best first step.
- Respect the existing design rules: no new accent color (`--now` stays reserved), and no change to the Level 1 crown, which should stay independent of any thread.

## Acceptance Criteria

- [ ] A controlled thread vocabulary is documented in `EVENT_CORPUS_GUIDE.md`
- [ ] The existing corpora are tagged against it (the verification pass in AE-15 is a natural time to do this)
- [ ] The live array carries thread membership
- [ ] A viewer can select a thread and see its events across epochs
- [ ] Verified in-browser at full zoom-out and at one zoomed-in level

## Non-Goals

- Doesn't change event levels or epoch boundaries
- Doesn't require every event to belong to a thread
- Doesn't need to wait for the remaining epochs, though a thread is more convincing the more epochs it spans

## Related

- First real cross-epoch element already in the instrument: the Cold War span (1947 to 1991), owned by `american_ascendancy_events.yaml` and documented as a deliberate exception in `EVENT_CORPUS_GUIDE.md`. It is a natural first user of whatever threads mechanism gets built.

- Prompted by: `src/data/return_to_normalcy_outline.md` (the immigration thread note)
- Overlaps: [AE-14](AE-14-epoch-handoff-field.md) (both expose intellectual succession) and [AE-15](AE-15-gilded-age-fact-verification.md) (the verification pass could also standardize tags)
