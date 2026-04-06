# ---

> OpenClaw AI Agent Skill

---
name: systematic-debugging
description: 4-phase root cause investigation for any bug, test failure, or unexpected behavior. Use when encountering ANY technical issue — bugs, crashes, test failures, performance problems, integration errors. Enforces finding root cause BEFORE attempting fixes. NO random fixes allowed.
---

# Systematic Debugging

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

Random fixes waste time and create new bugs. Symptom fixes mask underlying issues.

## Phase 1: Reproduce

**Goal:** Confirm the bug exists and get exact reproduction steps.

1. Get the EXACT error message, stack trace, or unexpected behavior
2. Find the MINIMAL reproduction case — strip away everything non-essential
3. Confirm it's reproducible (not a flaky/timing issue)
4. Document: input → expected output → actual output

**Output:** Clear reproduction steps anyone can follow.

**If you can't reproduce:** Gather more context. Check logs, environment differences, timing. Do NOT guess.

## Phase 2: Isolate

**Goal:** Narrow down to the specific component, file, and line.

1. **Binary search the codebase** — comment out half, does it still fail?
2. **Check recent changes** — `git log --oneline -20`, `git diff HEAD~5`
3. **Trace the data flow** — follow the input from entry point to error
4. **Add targeted logging** — not everywhere, just at decision points
5. **Check boundaries** — input validation, type mismatches, null/undefined

**Output:** "The bug is in [file], [function], around line [N], triggered when [condition]."

## Phase 3: Root Cause

**Goal:** Understand WHY it breaks, not just WHERE.

Ask these questions:
- **Why does this code exist?** What was the original intent?
- **What assumption is violated?** Every bug is a broken assumption
- **Is this a design flaw or an implementation bug?** Design flaws need different fixes
- **Are there other places with the same bug?** Pattern-match across the codebase
- **What changed?** Environment, dependency, data, config, or code?

**Output:** "The root cause is [X] because [Y]. It was introduced by [Z]."

## Phase 4: Fix

**Only after completing Phases 1-3.**

1. Write the fix targeting the ROOT CAUSE (not the symptom)
2. Verify the original reproduction case now passes
3. Check for regressions — did the fix break anything else?
4. Add a test that catches this specific bug (prevent recurrence)
5. Document what happened and why (for future reference)

**Anti-patterns to avoid:**
- ❌ "Let me try this" without understanding the problem
- ❌ Fixing the symptom instead of the cause
- ❌ Adding a special case / hack instead of fixing the real issue
- ❌ "It works now" without understanding why
- ❌ Skipping the regression check

## Quick Reference

| Phase | Question | Output |
|-------|----------|--------|
| Reproduce | Can I make it fail reliably? | Exact reproduction steps |
| Isolate | Where exactly is it breaking? | File, function, line |
| Root Cause | Why is it breaking? | The broken assumption |
| Fix | How do I fix the actual cause? | Tested fix + regression check |

## When to Escalate

If after 30 minutes of investigation you can't isolate:
- Ask for help — describe what you've tried and ruled out
- It may be an environment issue, not a code issue
- It may be a race condition or timing-dependent bug (hardest to debug)

## Installation

```bash
cp -r systematic-debugging/ ~/.openclaw/workspace/skills/systematic-debugging/
```

## License

MIT © [Sentra Technology](https://github.com/Icattj)
