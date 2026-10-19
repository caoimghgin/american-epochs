# Event Corpus Guide

Format and curation spec for researching one epoch's events, so the work can be
farmed out to other LLMs (or humans) without them needing this project's full
chat history — just this file, `CLAUDE.md`, `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md`,
and `house_divided_events.yaml`/`reconstruction_events.yaml` as worked examples.

Read `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` first if you haven't — it's the
*why* behind everything in this file (progressive disclosure as the design
unit, not the event; the event-selection tests; the recurring American
question this whole project keeps circling back to). This guide is the
*how*: schema, thresholds, sourcing, reconciliation mechanics.

## What this is, and isn't

Each file here is a **research and authoring artifact** for one epoch of
American Epochs. It is not loaded by the instrument at runtime — the project
has zero build tooling and no YAML parser by design (see `CLAUDE.md`). A human
or LLM reconciles a finished corpus by hand into the inline `EVENTS` array in
`src/american-epochs.html`. Producing a well-researched, correctly-leveled
YAML file is the whole deliverable; wiring it into the live instrument is a
separate, later step.

## Naming convention

`src/data/<epoch_id>_events.yaml`, where `<epoch_id>` matches the epoch's `id`
in the live `EPOCHS` array (e.g. `hdiv` → `house_divided`, spelled out rather
than abbreviated for readability). One file per epoch.

## Schema

Every file starts with a `schema:` block (name, version, `level_definition`,
`curation_guidance`, `notes`) — copy it from `house_divided_events.yaml`
verbatim; it's project-wide guidance, not epoch-specific. Then an `epoch:`
block (`id`, `title`, `start`/`end` as `YYYYMMDD`, `thesis` — one or two
sentences on what the epoch is *about*, matching the tone of its `CLAUDE.md`
entry). Then `events:`, a flat list. Each event:

```yaml
- id: kansas_nebraska_act        # snake_case, unique within the file
  start: 18540530                # YYYYMMDD; unknown month/day just use the
                                  # 1st, but see "date precision" below
  end: 18581231                  # omit entirely for a point event; present
                                  # only for genuine multi-day/week spans
  level: 1                       # 1-5, see level_definition and curation
                                  # guidance below — this is the hard part
  kind: legislation               # free-text category (legislation, battle,
                                  # speech, election, court_case, ...) — not
                                  # enforced, but keep it consistent within a
                                  # file so a later pass can group by kind
  title: Kansas–Nebraska Act
  description_short: One sentence, plain factual summary.
  description_long: Two or three sentences of context — what happened, who
    was involved, what immediately followed. This is research depth; it is
    NOT what ends up in the live instrument's tooltip.
  why: One sentence on why this event matters to the epoch's own argument —
    this is the interpretive claim, and it's what the live `note` field gets
    distilled from during reconciliation.
  tags: [slavery, federalism, congress, territories]
  date_precision: year           # optional: year | month. Omit for a full day-level date.
                                  # When set, start holds a placeholder (the 1st) that
                                  # reconciliation must NOT render as an exact date.
  date_verified: true            # optional: set only once the date is confirmed against
                                  # an identifiable source
  sources:                        # omit the whole key if nothing solid exists
    - name: Library of Congress
      url: https://...
```

## The level system — read this part carefully

Levels aren't a difficulty slider. They're what the live instrument uses to
decide what's visible at a given zoom: **Level 1 is always on screen, even at
the widest possible view of all 172 years of American history at once.**
Levels 2–5 reveal automatically as the viewer zooms in — never a manual
toggle, never a viewer choice. Getting Level 1 wrong makes the whole
instrument's argument at its most-seen moment wrong.

**Target 5–8 Level 1 point events, regardless of epoch length or how
eventful it was.** This is the single most important curation call in the
file, and it does not get easier for a longer or bloodier epoch — a longer
epoch just means harder cuts, not a bigger crown. Ask of each candidate: does
removing this break the causal chain from the epoch's start to its end? If
the answer is no, it's a Level 2 (or lower) event, however famous it is.

A House Divided's own Level 1 went through two rounds of trimming (17 → 10 →
7 point events) before landing on: Kansas-Nebraska Act, Lincoln elected, Fort
Sumter, Emancipation Proclamation, Gettysburg, Appomattox, Lincoln
assassinated. Dred Scott, John Brown's raid, and South Carolina secedes are
all genuinely important and all got cut on the second pass — they're real
causes, not hinges the arc turns on. Use this as the calibration example:
if your Level 1 list looks more like "everything a textbook chapter title
would mention" than "the seven beats you'd tell a friend in one breath,"
it's still too big.

Span (duration) events can also carry `level: 1` for scene-setting context
(e.g. `bleeding_kansas`, a years-long span) — these don't count against the
5–8 point-event target, because the live instrument never tier-filters spans;
they render unconditionally in the Duration strip at every zoom regardless of
level. Level 1 span entries are additional context, not part of the
point-event crown a viewer sees pop onto the timeline.

Levels 2–5 have far more room to breathe. Disagreement there is a rounding
error, not a structural problem — spend the real editorial effort on Level 1.

**5–8 is a default, not a hard cap.** Reconstruction's Level 1 intentionally
carries nine point events, not seven or eight, because the Panic of 1873 and
*United States v. Cruikshank* mark genuinely distinct mechanisms of retreat
(economic collapse vs. judicial narrowing) — collapsing them into one entry
would blur two different causal stories into a false single one. The bar for
exceeding the default is explicit justification written into the corpus
(`reconstruction_events.yaml`'s own `curation_guidance` says so directly, and
`reconstruction_outline.md` spells out why), not just "this epoch felt like
it needed more." An undocumented 9-event crown is scope creep; a documented
one is an editorial decision someone can disagree with on the merits.

## Span vs. point events

Record the true factual date range in this YAML regardless of how it will
eventually render — `start`/`end` here is a research question ("how long did
this actually last"), not a display question. A months-long campaign really
did last months; write that down even if it won't end up as a Duration-strip
bar (see below).

**The live instrument's Duration strip is reserved for genuinely long-running
phenomena — roughly two years or more.** Anything shorter renders as a point
event positioned at its **end date**, not its start, because the end is
usually the moment the event actually resolves into something (a campaign
*culminates* in a surrender or a capture; a crisis *fails* or *concludes*).
This is a reconciliation-time decision, made when merging into
`american-epochs.html`, not something this YAML needs to anticipate — but if
you already know an entry's duration is well under two years, don't be
surprised when it surfaces as a point event later rather than a span.

**Duration alone is necessary but not sufficient.** Clearing two years earns
an entry a look, not a place. The Duration strip is always-on, at every zoom
— the same "does the viewer need to see this whether they zoom in or not"
bar as Level 1, just for conditions instead of moments. Reconstruction's
first full reconciliation gave five entries span treatment purely because
they cleared the threshold (a federal bureau's operating years, a bank's
charter-to-failure span, a political-club's membership window) — Kevin
caught it immediately on sight: real institutions, but institutional facts
that happen to have a date range, not background conditions the whole epoch
turns on. `reconstruction_outline.md` had already named the epoch's intended
long-running conditions, and the extra five just hadn't been checked against
that list before reconciling. Write down the real duration in the YAML
regardless; decide separately, against the epoch's own outline if one
exists, whether it's earned the permanent lane.

Even an outline's own named list isn't the final word — a second look cut
"Congressional Reconstruction" itself (~10 of the epoch's ~12 years) once the
other five were gone, because at that length it read as barely distinct from
the epoch band's own boundaries, not a phase earning a separate always-on
mark. Its sibling "Presidential Reconstruction" (~2 years) stayed, because it
genuinely *contrasts* with what follows rather than just relabeling most of
the epoch. The test isn't "is this a named, bounded period" — it's "does the
viewer learn something by seeing this marked as distinct from the epoch
itself, at every zoom, forever." A span covering most of an epoch usually
fails that test even when it clears every other bar.

**Cross-epoch spans are a deliberate exception.** The Cold War (1947 to 1991) crosses
five epochs, and the guide's rule against spans that cover most of an epoch exists to
stop a span from merely relabeling that epoch. A span that belongs to no single epoch
and holds several together does a different job: the viewer learns that one condition
runs under all of them. Such a span is owned by the corpus where it begins, carries its
true full dates in the YAML and the live array, and its ending event is carried by the
corpus where it ends. It is the first real cross-epoch element in the instrument, and a
natural first user of the threads idea in `tickets/AE-16-cross-epoch-threads.md`.

This threshold came from direct experience: A House Divided's first pass at
reconciliation used a much shorter cutoff (14 days) and ended up with over a
dozen short campaigns and crises cluttering the Duration strip at moderate
zoom — a multi-month campaign and a three-day battle read almost the same
width on screen, which defeats the point of having a separate lane for
duration at all. Two years is deliberately conservative: it's reserved for
things a viewer should perceive as *background conditions* (a war, a
depression, a scandal that drags on), not events that merely took more than
a week to play out.

## Date precision — no fabrication

Use the most precise date you can actually verify (year, or year+month, or
full year-month-day). Never invent a day or month to make an entry look more
precise than the research supports — a plain year is fine and expected for
some entries, especially in Levels 3–5. This matches the project's honesty
bar elsewhere (the register system, the crime-lane coverage notes in
`CLAUDE.md`): don't manufacture certainty that isn't there.

## Sourcing

Prefer primary/authoritative public sources: Library of Congress, National
Archives, National Park Service, the House/Senate historical offices,
Smithsonian, or a specific academic institution's published material. One or
two sources per event is plenty — this isn't a bibliography. If nothing
solid and specific is readily available, omit the `sources` key entirely
rather than inventing or hand-waving a citation.

## Epoch-boundary events

Some events sit right on the seam between two epochs (Lincoln's
assassination, four days after Appomattox and days into what becomes
Reconstruction, is the clearest example — the live instrument counts it as
A House Divided's own closing beat; an early Reconstruction draft once
counted it too, before the duplicate was caught and removed during
reconciliation). When a candidate event could plausibly belong to either
neighbor, make an explicit call and say why in its `why` field, rather than
silently including it in both files or dropping it from both.

## Optional: a per-epoch outline

For an epoch this size or larger, it's worth writing a short companion
`<epoch_id>_outline.md` alongside the YAML, produced by the Editorial Review
sequence in `FOUNDATIONAL_EDITORIAL_PRINCIPLES.md` (outline the story →
build an unfiltered candidate list → cluster conceptually → choose Level 1 →
draft the handoff → write YAML). See `reconstruction_outline.md` for most of
the pattern: historical thesis, foundational question, a big-picture story
in a paragraph, an act structure if the epoch has one, an explicit Level 1
spine with reasoning, and boundary decisions — it predates the epoch-identity
paragraph and handoff sentence the process now also calls for, so treat
those two as additions rather than expecting to find them there yet. This
isn't required by the schema, but it's where the *argument* for a corpus's
editorial choices lives — the YAML says what the Level 1 events are; the
outline says why those nine and not a different nine.

## Reconciliation (what happens to this file later)

This YAML does not get parsed by the page. Merging it into
`src/american-epochs.html`'s `EVENTS` array involves real editorial
compression, not a field-for-field copy:

- `start`/`end` (`YYYYMMDD`) → `y`/`month`/`day` and, for spans, `end`/
  `endMonth`/`endDay`
- `level` → `tier`
- `title` → `label`
- `description_short` + `description_long` + `why` → a single `note`: one
  crisp interpretive sentence in the voice already used throughout
  `american-epochs.html`, not a concatenation of the YAML's three fields
- `kind`, `tags`, and `sources` don't carry over to the live event object —
  they're research/organization scaffolding for this file, not runtime data

## Status across epochs

| Epoch | File | Status |
|---|---|---|
| A House Divided (1854–1865) | `house_divided_events.yaml` | Done — 5-level, reconciled into the live instrument |
| Reconstruction (1865–1877) | `reconstruction_events.yaml` | Done — 114 events, 5-level, reconciled into the live instrument (see `reconstruction_outline.md`) |
| The Gilded Age (1877–1897) | `gilded_age_events.yaml` | Done, first pass — 118 events (8/15/25/47/21 points + 2 spans, after a gap-fill pass on sparse years), 5-level, reconciled into the live instrument (see `gilded_age_outline.md`). Dates drafted from memory, not yet source-checked |
| The Big Stick (1898–1912) | `big_stick_events.yaml` | Done, first pass — 114 events (8/14/23/53/15 points + 1 span), 5-level, reconciled into the live instrument (see `big_stick_outline.md`). Dates drafted from memory, not yet source-checked |
| The New Freedom (1913–1919) | `new_freedom_events.yaml` | Done, first pass — 105 events (7/16/25/44/12 points + the World War I span), 5-level, reconciled into the live instrument (see `new_freedom_outline.md`). Dates drafted from memory, not yet source-checked |
| Return to Normalcy (1920–1932) | `return_to_normalcy_events.yaml` | Done, first pass — 100 events (8/12/24/41/14 points + the Great Depression span), 5-level, reconciled into the live instrument (see `return_to_normalcy_outline.md`). Dates drafted from memory, not yet source-checked |
| A New Deal (1933–1940) | `new_deal_events.yaml` | Done, first pass — 105 events (7/15/24/45/13 points + the Dust Bowl span), 5-level, reconciled into the live instrument (see `new_deal_outline.md`). Dates drafted from memory, not yet source-checked |
| The American Ascendancy (1941–1952) | `american_ascendancy_events.yaml` | Done, first pass — 106 events (9/14/33/39/8 points + three spans: World War II, the Korean War, and the Cold War), 5-level, reconciled into the live instrument (see `american_ascendancy_outline.md`). Dates drafted from memory, not yet source-checked |
| The Affluent Society (1953–1965) | `affluent_society_events.yaml` | Done, first pass — 117 events (8/17/37/42/12 points + the Vietnam War span), 5-level, reconciled into the live instrument (see `affluent_society_outline.md`). Dates drafted from memory, not yet source-checked |
| The Unravelling (1966–1979) | `unravelling_events.yaml` | Done, first pass — 121 events (8/21/34/42/14 points + the Watergate and Stagflation spans), 5-level, reconciled into the live instrument (see `unravelling_outline.md`). Carries the end events of the Vietnam War span. Dates drafted from memory, not yet source-checked |
| Morning in America (1980–1991) | — | Not started |
| The End of History (1992–2007) | — | Not started |
| The Interregnum (2008–2015) | — | Not started |
| The Reckoning (2016–present) | — | Not started |

Tracked under ticket AE-10 (`tickets/AE-10-event-date-backfill.md`) — one
epoch at a time, no required order, no deadline.
