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

**Deriving the structure.** Don't organize sections by how the investigation
happened to unfold — how many separate threads got pulled, or in what order
they were checked. Instead, find the actual logical or mathematical shape of
the problem, the formula or model the findings satisfy, and let that set the
hierarchy. If several findings are really sub-cases of one underlying factor
(e.g. a system's output is headcount × per-worker speed, and two different
findings both just change "speed"), nest them under that factor instead of
listing them as unrelated peers.

Group sections by what the thing *is* (the logical subject — a lever, a
component, a decision), never by its status (done vs. open, shipped vs.
not). Status is a label inside a section, not the axis that splits the
document — otherwise one subject ends up split across unrelated top-level
sections depending on which part of it happened to ship. If there are
several open items worth tracking, add one short summary section at the end
that recaps just those: a recap layered on top of the logical structure, not
a replacement for it.

Once a section tree has more than about 5 items at one level, group related
ones under a higher-level heading so the document's overall shape is visible
at a glance. A group heading alone isn't top-down, though — follow it with a
short intro that states what relates the items before listing them, and say
which items share that relationship and which don't, rather than stretching
one frame to cover something it doesn't actually explain.

## Step 3 — Write, banning activity-log style

- **Never** narrate the process: no "I investigated...", "I then checked...",
  "turns out I was wrong about X, so I re-checked...", "after running Y I
  found...". This also covers narrating the doc's own structure or ordering
  ("covered below in that order, starting with X since it's the smaller
  change") — that's a drafting decision, not something the reader needs. In
  both cases, the reader does not care about the path to the answer, only
  the answer; let the structure speak for itself.
- Lead each section with the conclusion, not the method. Evidence or method
  goes in a line or two underneath if it matters to the audience, not as the
  main narrative.
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
  instead, picking whichever actually fits the logical relationship. An en
  dash or hyphen inside a number range (e.g. "27–31M", "12–24h") is unrelated
  and fine to keep.

### Evidence and confidence — don't launder a guess into a fact

Every non-obvious claim needs to show where it came from: the metric, log,
or test that supports it, with the actual numbers.

- State the measured fact together with its evidence, not the conclusion
  alone with no trail back to how it was established. Cite it so it's
  reproducible: name the actual metric/log/query (namespace, dimensions,
  stat, aggregation) behind a number, not just the number with a vague
  parenthetical. When the source has no shareable link, give the exact
  query parameters so someone else could pull the same number themselves —
  don't fabricate a link that might point to the wrong account, region, or
  time range.
- When the *mechanism* behind a measured effect isn't itself directly
  confirmed, say so. "Changing X produced effect Y, consistent with Z" is
  honest; "X happens because Z" is not, unless Z was actually isolated and
  alternatives were ruled out.
- State what's still unconfirmed or unknown as its own line; don't quietly
  drop it to make the doc read more confidently. Name the specific thing
  that wasn't done ("haven't profiled per-operation CPU time"), not vague
  language like "uncertain." Then weight it correctly: call it a quick thing
  nobody got to yet, or **pending further investigation** if resolving it
  actually requires real additional work — don't undersell a real gap as a
  trivial oversight.
- Testing only the thing that was easiest to test doesn't make it the
  answer. If a convenient benchmark or environment doesn't match production
  conditions, say what the gap is instead of letting the reader assume it
  doesn't exist.
- A question worth investigating is not automatically worth writing up. The
  bar for including a checked side-question is "does this matter enough to
  the reader's decision, or add enough real complexity, to earn the space,"
  not "was there a problem to check." A side-check that turns out low-stakes
  and adds no real complexity stays out, same as one never asked; one that
  turns out to matter goes in regardless of how much complexity it adds. The
  exception: a cleared side-check that a reader would plausibly ask about on
  their own ("why not just tune X?") earns a one-line ruled-out note, placed
  with other ruled-out options rather than as an open item — reserve this
  for questions an obvious reader would actually raise, not a manufactured
  rebuttal nobody would think to ask.
- A brief dismissal used to justify focusing on one option over sibling
  options ("the others aren't realistic, because...") makes a claim like any
  other and needs the same evidence bar, not a free pass because it reads as
  throat-clearing. If it wasn't checked, say that, rather than asserting
  something sounds obvious.
- When a doc compares multiple options or levers, check cost for all of
  them, not just the ones where it was easy to quantify. If cost shows up
  for one option and is silently missing for a sibling, the omission itself
  reads as "no cost here" even when the truth is "not checked" — that's
  misleading by omission, not neutral silence. State the cost angle for
  every option, even when the honest answer is "direction unclear, hasn't
  been measured."

### Analogies — two narrow uses, otherwise skip

Default to no analogy; most docs don't need one. Use one only when:

- **The point itself is non-obvious**, not just unfamiliar vocabulary — a
  fact the audience could just as well hear in plain technical terms doesn't
  qualify; that's a vocabulary gap, fixed with the correct term, not a
  metaphor. A real candidate is an insight that would still be surprising to
  an expert in the subject.
- **Several individually-obvious findings have a non-obvious relationship**
  — one analogy mapping each finding onto part of a single scene can make
  that relationship click where a bullet list wouldn't (e.g. mapping
  headcount, per-worker speed, and per-unit work onto one production line).

Pick exactly one grounded, ordinary scene per doc — never stack comparisons
— sized only to carry the specific insight. Default to introducing it once,
as a closing or organizing device, not sustained across every paragraph.

The exception is a doc built around several parallel levers that all map
onto the same scene (the second case above). There, reusing the scene once
per lever, as that section's opening frame before the technical detail, is
fine — but every reuse has to carry real explanatory weight: map that
lever's actual mechanism into the scene's own terms, so the analogy does the
explaining. Don't just restate the mapping as a label and then revert
straight to technical prose. A scene-sentence that doesn't actually change
how the mechanism gets explained is worse than no touch at all: cut it
rather than padding every section with a label.

## Then publish

Follow this session's normal Notion / Artifact conventions for where the doc
goes (ask for a destination if none was given, default to a private draft).
Before handing over the link, re-read the draft once specifically checking
for leftover process-narration language ("I", "then", "turns out",
"after checking") and cut it.
