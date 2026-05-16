# Discuss — Gray Area Resolution

Resolve ambiguous requirements before design begins. Capture user decisions in `context.md`.

## When This Phase Triggers

Triggered automatically from SPECIFY when the agent detects:

- UX behavior that depends on user preference (layout, interaction flow, API response shape)
- Two valid interpretations of a requirement with different implementation costs
- Decisions that affect bounded context boundaries
- Security or compliance choices that need explicit sign-off
- Performance trade-offs where the right answer depends on usage patterns

**Scope guardrail:** Discuss is for clarifying HOW, never for changing WHETHER. The feature scope is fixed by the spec. Do not introduce new requirements here.

## Protocol

Present gray areas in domain-specific terms — never in implementation terms. The user cares about behavior, not code.

**Structure each gray area as:**

```
**[Area N]: {Short title}**

Situation: {what the spec leaves ambiguous}

Option A: {behavior/outcome}
→ Implication: {trade-off in user terms}

Option B: {behavior/outcome}
→ Implication: {trade-off in user terms}

Your preference?
```

- Present 2-4 gray areas per round, then wait for answers
- After answers, check if new gray areas emerged from the decisions
- Never present more than 4 at a time — cognitive overload kills quality decisions
- Follow up with clarifying questions when an answer is still ambiguous

## Output

Save decisions to: `{artifacts.path}/specs/{feature-slug}/context.md`

```markdown
# Context: {Feature Title}

**Date:** {YYYY-MM-DD}
**Status:** Locked

## Decisions

### [Area 1]: {Short title}
**Decision:** {User's choice, paraphrased precisely}
**Implication:** {What this means for design/implementation}
**Rationale:** {Why the user chose this, if stated}

### [Area 2]: {Short title}
**Decision:** {User's choice}
**Implication:** {What this means}
```

Mark `context.md` as **Locked** once all gray areas are resolved. Do not reopen it during DESIGN or PLAN — if new ambiguity emerges, add a new section and seek approval.

## After Resolution

- `context.md` is locked
- Load it into context for every subsequent phase (DESIGN, PLAN, EXECUTE)
- Proceed to [design.md](design.md) or [plan.md](plan.md) depending on complexity
