# State Management — Persistent Project Memory

`.specforge/STATE.md` is the project's long-term memory. Updated by REVIEW after each PR. Read by every agent before executing.

## File Location

`.specforge/STATE.md` in the project root.

Initialize with `templates/state-template.md` when setting up a new project.

## Structure

```markdown
# Project State

## Decisions

Architecture and product decisions. Accumulated over time.

### AD-{NNN}: {Short title}
**Date:** {YYYY-MM-DD}
**Context:** {What forced this decision}
**Decision:** {What was decided}
**Rationale:** {Why}
**Consequences:** {What this enables or constrains}

---

## Blockers

Active blockers that affect ongoing work.

### B-{NNN}: {Short title}
**Status:** Active | Resolved
**Opened:** {YYYY-MM-DD}
**Resolved:** {YYYY-MM-DD or —}
**Description:** {What is blocked and why}
**Impact:** {Which features or tasks are affected}
**Resolution:** {How it was resolved, or current workaround}

---

## Lessons

Registered by REVIEW after each PR. Read by all agents before writing code.

### L-{NNN}: {Short title}
**Date:** {YYYY-MM-DD}
**Category:** CodeQuality | Architecture | Performance | Security
**Error:** {What was done wrong}
**Why it's wrong:** {The principle or invariant violated}
**Correct approach:**
```{language}
{correct code snippet}
```
**Area:** {bounded context or cross-cutting}

---

## Deferred Ideas

Good ideas captured during spec/discuss/review that are out of scope for now.

- [ ] {YYYY-MM-DD} {idea description} — captured during {spec/review/discuss} of {feature}

---

## Todos

In-progress and pending items.

- [ ] {item}
- [x] {YYYY-MM-DD} {completed item}

---

## Preferences

Model guidance preferences accumulated over sessions.

- {preference note}
```

## When to Update Each Section

| Section | Updated by | When |
|---|---|---|
| Decisions | DESIGN, SPECIFY | When an architectural or product decision is made |
| Blockers | Any phase | When a blocker is identified or resolved |
| Lessons | REVIEW | After every PR review, for every blocking/important finding |
| Deferred Ideas | SPECIFY, DISCUSS, REVIEW | When scope creep is captured but not acted on |
| Todos | Any phase | When a task or follow-up is identified |
| Preferences | REVIEW | When model behavior guidance is noted |

## Counter Management

Counters (AD-NNN, B-NNN, L-NNN) are monotonically increasing. Before adding a new entry:
1. Read the last entry of that type
2. Increment by 1
3. Never reuse a number, even if entries are deleted

Start at 001 for each counter.

## Loading Context

Agents load STATE.md at the start of each phase. Key reading strategy:
- **Lessons section** — always read before writing code
- **Decisions section** — read before any architectural choice
- **Blockers section** — check Active blockers before starting work
- **Deferred Ideas** — check before SPECIFY to avoid duplicating ideas as new features
