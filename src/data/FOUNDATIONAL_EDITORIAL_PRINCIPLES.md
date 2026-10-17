# Foundational Editorial Principles

## Purpose

American Epochs is not another historical timeline.

Most timelines answer *what happened*. Good history books answer a second
question, *why did it happen*. This project attempts a third:

> **What question was America trying to answer during this period?**

That distinction changes everything. Rather than a sequence of disconnected
events, each epoch becomes part of a single constitutional conversation that
stretches across the nation's history — one generation inherits an unresolved
question, proposes an answer, and that answer becomes the next generation's
question in turn.

The unit of design is not the event. The unit of design is understanding.

## Epoch Identity

Before any analytical breakdown, every epoch needs one thing a contributor
can hold in their head while researching it: a single paragraph answering —

> If someone remembered only one thing about this period fifty years from
> now, what should it be?

Not the thesis (below) and not the foundational question. The identity is
more compressed and more visceral than either — closer to what the epoch
*feels like* in retrospect. For the Gilded Age, something like:

> The Gilded Age was America's first encounter with the promises and dangers
> of industrial capitalism. Extraordinary innovation created unprecedented
> wealth, but it also forced the nation to confront whether democratic
> institutions could govern an economy operating at continental scale.

Write this first, before event research begins. It's the sentence every
later curation call gets checked against.

## The Three Layers of Every Epoch

Once identity is established, every epoch should be understood through three
complementary, more analytical lenses.

### 1. Historical Thesis

What objectively changed during this period? This is the historian's
summary — it explains *what happened*.

> **The Gilded Age** — Industrial capitalism transformed the United States
> faster than its political institutions could adapt.

### 2. Foundational Question

Every epoch wrestles with a deeper constitutional, political, or
philosophical question. The question is not necessarily answered within the
epoch — it's the central problem the period organizes itself around, and it's
editorial guidance: it explains why seemingly unrelated events belong to the
same story.

> **The Gilded Age** — Who governs an industrial republic: citizens, parties,
> courts, or capital?

Stress-test any candidate question before committing to it: does every major
event you're considering actually fit under it? If not, the question is
wrong, not the events.

### 3. Historical Answer

Every period eventually produces an answer — not necessarily the *correct*
answer, simply the one history delivered. This answer becomes the inherited
condition of the next epoch.

> **The Gilded Age** — Industrial capitalism becomes the dominant organizing
> force while democratic institutions struggle to recover public control.

## The Handoff

Because every historical answer becomes the next epoch's question, that
inheritance deserves to be written down explicitly — not as a summary of what
just happened, but as a bridge to what comes next. One or two sentences,
ending an epoch's outline, that could plausibly open the next epoch's:

> *The Constitution had been transformed, but the nation's economic
> transformation was only beginning. As railroads, corporations,
> immigration, and industrial capital reshaped American life, the country's
> next great question would no longer be who possessed rights — but who
> possessed power.*

That's Reconstruction's handoff into the Gilded Age. Every future outline
should draft one of these before event research begins — it's a cheap way to
check that the epoch's *Historical Answer* actually connects to its
successor's *Foundational Question*, rather than the two epochs reading as
independent essays.

Surfacing handoffs inside the live instrument itself (as a rendered
transition between epoch bands, not just outline prose) is real
data-model-and-UI work, tracked separately rather than folded into every
epoch's authoring pass.

## History as an Ongoing Conversation

American history does not progress through isolated eras. Each epoch
inherits unresolved problems from its predecessor; every historical answer
creates the next historical question:

```text
The Revolution
│
├── Where does authority come from?
▼
A House Divided
│
├── Can liberty survive slavery?
▼
Reconstruction
│
├── Who possesses rights?
▼
The Gilded Age
│
├── Who governs industrial society?
▼
...
```

The timeline's job is not simply to show chronological succession. It is to
show intellectual succession. Viewed this way, events that normally appear
unrelated become coherent — Standard Oil, Tammany Hall, the Pullman Strike,
Chinese Exclusion, the Sherman Antitrust Act, and the Farmers' Alliances stop
being isolated topics from a textbook chapter and become different attempts
to answer the same question: who actually governs an industrial republic?

## Implications for Event Selection — The Editorial Test

Every event should earn its place by helping answer one of three questions:

1. Does it advance the epoch's historical thesis?
2. Does it illuminate the epoch's foundational question?
3. Does it explain the historical answer that ultimately emerged?

If it does none of these, it probably belongs elsewhere, or not at all.
Historical importance alone is not enough — fame and category-coverage are
both insufficient on their own. Exhaustive completeness is not the goal;
coherence is.

Before adding any event, ask:

> If I removed this event, would the viewer understand the epoch less
> completely?

Before promoting an event to Level 1, ask a harder version of the same
question:

> If I removed this event, would the story itself break?

Only events that pass the second test belong on the narrative spine.

## Progressive Disclosure

The five-level hierarchy (spine → major explanation → mechanism → context →
discovery) is not a difficulty slider sorting facts by importance. Each
level does a different explanatory job:

- **Level 1 — Narrative spine.** The smallest collection of events capable
  of telling the epoch's story at the widest possible view.
- **Level 2 — Major explanation.** Why did the turning points occur?
- **Level 3 — Mechanism.** How did institutions, people, and policies
  produce those changes?
- **Level 4 — Context.** Political, social, economic, and cultural texture.
- **Level 5 — Discovery.** The surprising details that reward curiosity
  without obscuring the larger narrative.

Orientation comes first; explanation follows. The experience of zooming in
should feel like increasing understanding, not accumulating facts. The exact
numeric targets, span-vs-point mechanics, and reconciliation rules for
turning this into a working corpus live in `EVENT_CORPUS_GUIDE.md` — this
document is the *why*, that one is the *how*.

## The Editorial Review

Every epoch, without exception, goes through the same review before a single
line of YAML gets written. This is a process, not a person — it should
survive any specific contributor, so a reviewer five years from now
understands they're performing a role the project defines, not deferring to
whoever happens to hold it today.

The production sequence, validated by how Reconstruction was actually built:

1. **Outline the story.** Write the Epoch Identity, Historical Thesis,
   Foundational Question, and a big-picture story in a paragraph or two,
   before touching dates or YAML. Stress-test the foundational question
   against candidate events; if it doesn't explain most of them, revise it.
2. **Build the candidate list, unfiltered.** Gather every plausible
   event — laws, strikes, elections, court cases, breakthroughs,
   assassinations, companies — without curating yet. Completeness at this
   stage is the point.
3. **Cluster conceptually, not chronologically.** Group the candidate list
   by theme (e.g. Corporate Power, Labor, Politics) rather than by date.
   Only once the whole conceptual landscape is visible does it become
   possible to ask which events belong on the narrative spine — clustering
   first prevents Level 1 from becoming "whatever happened to occur early."
4. **Choose Level 1, and expect to argue about it.** This is where the real
   editorial effort belongs. Disagreement here is the valuable kind of
   disagreement; disagreement at Levels 2–5 is a rounding error by
   comparison.
5. **Draft the handoff** connecting this epoch's Historical Answer to the
   next epoch's Foundational Question.
6. **Write the YAML** per `EVENT_CORPUS_GUIDE.md`'s schema, now that the
   narrative architecture is settled.

An outline produced by steps 1–5 is worth writing down as its own file (see
`reconstruction_outline.md`) — it's where the *argument* for a corpus's
editorial choices lives, not just its conclusions.

The review should also periodically step back from any single epoch's
history and interrogate the architecture itself: is Level 2 actually
distinguished from Level 3, or doing the same job twice? Are any Duration
spans doing two jobs at once? Have descriptions drifted from explanatory
into encyclopedic? Those structural observations improve every future
corpus, not just the one under review — they matter more than "you missed
event X."

## The Recurring American Question

A recurring argument in American history is:

> Who is recognized as a bearer of rights, and who has the authority to
> protect those rights?

The American proposition treats rights as belonging to humanity rather than
as privileges bestowed by tribal membership. Much of American history can be
read as the repeated conflict between that universal claim and narrower
definitions of who belongs within the protected political community.

## The Ultimate Goal

By the time a viewer reaches the present day, they should not merely know
more American history. They should recognize that America has spent nearly
250 years engaged in a continuous constitutional conversation: every
generation inherits unresolved questions, proposes answers, and those
answers create the questions inherited by the next generation.

A viewer leaving any single epoch should possess:

1. A concise story of what happened.
2. An understanding of why it happened.
3. A sense of the deeper American argument that gave those events lasting
   meaning.

Zooming should reward the viewer with better understanding, not merely more
facts. American Epochs is the story of that conversation.
