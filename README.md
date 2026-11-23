# audience-first-doc

A [Claude Skill](https://www.anthropic.com/news/skills) that forces Claude to identify who a doc, write-up, or report is actually for *before* drafting it — and bans the chronological "I checked X, then I found Y" narration that investigation write-ups tend to default to.

## What it does

1. **Identifies the audience** from context, or asks if it's unclear — never guesses.
2. **Figures out what that audience needs**: what decision they'll make, what's new to them, what they'd otherwise have to dig up themselves.
3. **Writes to those needs**, leading with conclusions instead of narrating the process that got there.

## Install

```
npx skills add jiananyim/audience-first-doc
```

Or copy `SKILL.md` into your project's `.claude/skills/audience-first-doc/` or your user-level `~/.claude/skills/audience-first-doc/`.
