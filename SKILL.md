---
name: audience-first-doc
description: >
  Use before writing any Notion doc, write-up, summary, investigation report,
  or proposal meant for someone other than the user to read (a teammate, a
  manager, a channel, "the team", an on-call handoff, etc.) — including when
  the user says things like "write a doc for this", "put this in Notion", or
  "summarize this for the team", or pastes a Slack thread and asks for a
  summary doc. Does NOT apply to a quick answer in chat, or to a
  plan/analysis the user asked for only for themselves. Forces audience
  identification before drafting, and bans chronological "what I did / what
  I found" narration in the output.
---

# Audience-first doc writing

Most investigation write-ups default to a chronological log of the work:
"I checked X, then I found Y, then I tried Z." That log is organized around
the *investigator's* process, not the *reader's* questions. Nobody reading a
doc cares what order you did things in — they care whether it answers what
they showed up asking.

This skill forces three steps, in order, before any doc content is written.

## Step 1 — Identify the audience

Look at the conversation for an explicit or implied recipient: a named
person, a team, a Slack channel, "whoever's on call next", a manager who
asked for a status update, etc.

- If the audience is clear from context, state it in one line before writing
  anything else (e.g. "Written for: the on-call engineer picking this up").
- If the audience is **not** clear, stop and ask the user with
  `AskUserQuestion` — do not guess. Offer concrete candidates drawn from the
  conversation (e.g. "your manager", "the #oncall channel", "whoever reviews
  this PR") rather than generic options like "technical/non-technical".

Do not proceed to Step 2 until the audience is pinned down.

## Step 2 — Figure out what that audience needs

For the specific audience from Step 1, work out:

- What decision or action will they take after reading this? (approve
  something, fix something, stop worrying about something, escalate
  something)
- What do they already know, and what's actually new or surprising to them?
  Don't re-explain context they already have.
- What's the one thing they'd be annoyed to have missed?
- What would they have to dig through chat/logs/tickets to get if this doc
  didn't exist? That's the content that earns its place.

This becomes the doc's outline — usually 3-6 sections framed as the
audience's own questions ("What broke", "Why", "What we're doing about it",
"What we need from you"), not as phases of an investigation.

Once there are more than about 5 sections, don't leave them as a flat list
of same-level headings — group related ones under a higher-level heading
(e.g. several open decisions nested under one "Open decisions" heading) so
the document's overall shape is visible at a glance, not just its parts.

A group heading alone isn't top-down — follow it with a short intro that
states what relates the items before listing them (this is often exactly
where the second analogy use above belongs). Don't force every item under
the heading into one shared frame if it doesn't actually fit one of them;
say which items share the relationship and which don't, rather than
stretching the frame to cover something it doesn't explain.

Group sections by what the thing *is* (the logical subject — a lever, a
component, a decision), never by its status (done vs. open, shipped vs.
not). Status is a label inside the section, not the axis that splits the
document — otherwise one subject (e.g. "compression") ends up split across
two unrelated top-level sections depending on which part of it happened to
ship, which breaks the logical shape for no reason. If the doc has several
open items worth tracking, add one short summary section at the very end
that recaps just those, for a reader who only wants the open list — but
that's a recap layered on top of the logical structure, not a replacement
for it.

Derive the categories themselves from the actual logical/mathematical shape
of the problem, not from how many separate threads got investigated. If
several findings are really sub-cases of one underlying factor (e.g.
"throughput = quantity × speed", where instance type and compression are
both just ways of changing speed), nest them under that factor — don't list
them as peers just because they happened to be explored as separate strands
of work. Before finalizing a grouping, ask what formula or model the items
actually satisfy, and let that model (not the order or separateness of the
investigation) set the hierarchy.

## Step 3 — Write, banning activity-log style

When drafting:

- **Never** narrate the process: no "I investigated...", "I then checked...",
  "turns out I was wrong about X, so I re-checked...", "after running Y I
  found...". The reader does not care about Claude's or the author's path to
  the answer, only the answer.
- Lead each section with the conclusion, not the method. Evidence/method goes
  in a line or two underneath if it matters to the audience, not as the main
  narrative.
- Cut anything that was only interesting to the person doing the
  investigation (dead ends, corrected mistakes, tool-by-tool blow-by-blow).
  If a correction changes the final answer, state the final answer — don't
  show the before/after.
- No draft/date/provenance meta-labels in the doc body ("Draft v2", "Updated
  2026-01-01", "Generated by Claude") — content only.
- Match depth and jargon to the audience from Step 1: a manager gets impact
  and asks, not query plans; an on-call engineer gets the runbook-relevant
  specifics.
- Avoid the em dash (—) as a sentence connector; it's one of the more
  recognizable AI-writing tells, and overusing it reads as generated rather
  than written. Use a period, comma, colon, semicolon, or parentheses
  instead, picking whichever actually fits the logical relationship (a
  period for two separate claims, a colon when the second half explains or
  lists the first). An en dash or hyphen inside a number range (e.g.
  "27–31M", "12–24h") is unrelated and fine to keep.

### Evidence and confidence — don't launder a guess into a fact

Every non-obvious claim needs to show where it came from — the
metric/log/test that supports it, with the actual numbers — not just the
conclusion on its own.

- State the measured fact together with its evidence (e.g. "throughput rose
  ~1.6x — CloudWatch OffsetLag slope, before/after, on the three worst
  partitions"), not the conclusion alone with no trail back to how it was
  established.
- When the *mechanism* behind a measured effect isn't itself directly
  confirmed — profiled, isolated, or checked against alternative
  explanations — say so. "Changing X produced effect Y, consistent with Z"
  is honest; "X happens because Z" is not, unless Z was actually isolated
  and alternatives were ruled out.
- State what's still unconfirmed or unknown as its own line; don't quietly
  drop it to make the doc read more confidently. A real gap in the
  investigation is useful information for the reader, not something to
  paper over.
- Testing only the thing that was easiest to test doesn't make it the
  answer. If a convenient benchmark or environment doesn't match production
  conditions, say what the gap is instead of letting the reader assume it
  doesn't exist.
- A question worth investigating is not automatically worth writing up. The
  bar for including a checked side-question isn't "was there a problem" —
  it's "does this matter enough to the reader's decision, or add enough
  real complexity to the picture, to earn the space." A cleared side-check
  that turns out to be low-stakes and adds no real complexity stays out,
  the same as one that was never asked. A side-check that turns out to
  matter goes in regardless of how much explanatory complexity it adds.
- A checked, low-stakes finding isn't always silent, though: if it's a
  question the reader would plausibly ask on their own ("why not just tune
  X?"), a one-line ruled-out note saves them from redoing the same
  investigation — put it with other ruled-out options, not as an open item.
  Reserve this for questions an obvious reader would actually raise; don't
  manufacture a rebuttal for something nobody would think to ask.
- Don't flatten every open gap into vague language like "uncertain" or
  "not sure" — name the specific thing that wasn't done ("haven't profiled
  per-operation CPU time", "no rebalance event was traced"). Then pick the
  right weight for it: call it what it is if it's a quick thing nobody got
  to yet, but call it **pending further investigation** if resolving it
  actually requires real additional work — don't undersell a real gap as a
  trivial oversight.
- Cite evidence so it's reproducible, not just quoted. Name the actual
  metric/log/query (namespace, dimensions, stat, aggregation) behind a
  number, not just the number with a vague parenthetical. When the source
  has no shareable link (e.g. an internal metrics console), give the exact
  query parameters so someone else could pull the same number themselves —
  don't fabricate a link that might point to the wrong account, region, or
  time range.

### Analogies — two narrow uses, otherwise skip

Default to no analogy; most docs don't need one. Use one only when:

- **The point itself is non-obvious**, not just unfamiliar vocabulary — e.g.
  "a caught-up worker's steady-state throughput hides its real ceiling" is
  worth illustrating even for domain experts, because the insight holds
  regardless of their expertise. A fact the audience could just as well hear
  in plain technical terms ("it compresses the data") doesn't qualify —
  that's a vocabulary gap, fixed with the correct term, not a metaphor.
- **Several individually-obvious findings have a non-obvious relationship**
  — one analogy mapping each finding onto part of a single scene can make
  that relationship click where a bullet list wouldn't (e.g. mapping
  headcount, per-worker speed, and per-unit work onto one production line).

Pick exactly one grounded, ordinary scene per doc — never stack comparisons
— sized only to carry the specific insight, used once as a closing/
organizing device, not sustained across every paragraph.

## Then publish

Follow this session's normal Notion / Artifact conventions for where the doc
goes (ask for a destination if none was given, default to a private draft).
Before handing over the link, re-read the draft once specifically checking
for leftover process-narration language ("I", "then", "turns out",
"after checking") and cut it.
