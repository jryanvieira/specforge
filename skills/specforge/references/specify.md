# Specify — Requirement Capture

Transform a feature request, issue, or idea into a precise spec with traceable requirement IDs.

## Pre-Work

1. Load `.specforge/project.yaml` — extract `artifacts.path` and `issue_tracker.type`
2. Load `.specforge/STATE.md` — read lessons and decisions before assessing
3. Load `.specforge/architecture/` files if they exist — validate every decision against them
4. If `issue_tracker.type` is set, fetch the issue now (see [issue-tracker-adapters.md](issue-tracker-adapters.md))
5. Read `.specforge/PROJECT.md` — ensure the feature aligns with project vision

## Complexity Assessment

Before starting, classify the feature:

**Generate directly (no interview):**
- Issue already contains acceptance criteria, file paths, and expected behavior
- Single bounded context, no new interfaces
- Trivial complexity (≤5 requirements obvious from the issue)

**Conduct interview:**
- Multiple bounded contexts affected
- New interfaces or contracts
- Ambiguous requirements or conflicting user stories
- Medium/High complexity

## Interview Protocol

When interview is needed: **one question at a time**. Wait for the answer before asking the next.

Cover these topics — in the order that makes sense for this feature:

1. What exact problem does the end user face today?
2. What is the expected behavior after this feature?
3. Which bounded contexts are affected?
4. Are there changes to existing interfaces, or only additions?
5. What are the critical edge cases?
6. What are the testable acceptance criteria?
7. Are there security, performance, or compliance constraints?

Deepen each answer before moving on. Only advance when you have enough clarity to write Go contracts (or equivalent for the project stack).

**Gray area detection:** If during the interview you identify ambiguous UX behavior, conflicting requirements, or decisions that depend on user preference — do NOT resolve them yourself. Instead, trigger [discuss.md](discuss.md) to capture those decisions before proceeding.

## Spec Format

Save the spec to: `{artifacts.path}/specs/{feature-slug}/spec.md`

Use the template at `templates/spec-template.md` as base. All sections below are mandatory unless marked optional.

---

### Header

```markdown
# Spec: {Feature Title}

**Issue:** {tracker-type}-{number} | {url}
**Date:** {YYYY-MM-DD}
**Status:** Draft | Approved
**Complexity:** Quick | Simple | Medium | Complex
**Bounded Contexts:** {list}
```

### Problem Statement

One to three sentences. Describe the user problem, not the solution. Avoid implementation details here.

### Requirements

Each requirement gets a unique ID in the format `{PREFIX}-NNN` where PREFIX is a 3-4 letter abbreviation of the feature (e.g., `AUTH-001`, `GOAL-002`, `EXP-003`).

```markdown
## Requirements

### Functional

- **{PREFIX}-001** [P1] {Requirement description}
  - Acceptance: Given {context}, when {action}, then {outcome}

- **{PREFIX}-002** [P2] {Requirement description}
  - Acceptance: Given {context}, when {action}, then {outcome}

### Non-Functional (if applicable)

- **{PREFIX}-010** [P2] {Performance/security/reliability requirement}
  - Acceptance: {measurable criterion}
```

Priority levels:
- **P1** — must have (blocks ship)
- **P2** — should have (important but not blocking)
- **P3** — nice to have (deferred if needed)

### Bounded Contexts

List each affected context and describe the change scope:

```markdown
## Bounded Contexts

| Context | Impact | Change Type |
|---|---|---|
| {ContextName} | {what changes} | Addition / Modification / Read-only |
```

### Interfaces & Contracts (for backend features)

List the Go interfaces, types, and domain errors that must exist after implementation. Be precise — subagents will implement against these contracts.

```markdown
## Interfaces & Contracts

### {ContextName}

```go
type {Entity} struct {
    ID        string
    // ...
}

type {Repository} interface {
    Create(ctx context.Context, e *{Entity}) error
    FindByID(ctx context.Context, id string) (*{Entity}, error)
}

// Domain errors
var (
    Err{Entity}NotFound = errors.New("{entity} not found")
    Err{Entity}Invalid  = errors.New("{entity} invalid")
)
```
```

### Out of Scope

Explicitly list what this spec does NOT cover. Prevents scope creep during PLAN and EXECUTE.

```markdown
## Out of Scope

- {Thing that was considered but excluded}
- {Related feature that will be a separate spec}
```

### Risks

```markdown
## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| {risk description} | Low/Med/High | Low/Med/High | {mitigation} |
```

### Deferred Ideas

Capture good ideas that came up during the interview but are out of scope for this spec. They go into STATE.md as deferred ideas — not lost, just not now.

---

## Validation Checklist

Before marking the spec as Approved, verify:

- [ ] Every requirement has a unique ID and priority level
- [ ] Every acceptance criterion is testable (Given/When/Then format)
- [ ] All affected bounded contexts are identified
- [ ] Interfaces/contracts are typed correctly (centavos not floats, pointers for optionals)
- [ ] Domain errors are listed per context
- [ ] Out of scope section prevents ambiguity during PLAN
- [ ] No architectural decisions violate `.specforge/architecture/` guides
- [ ] Deferred ideas are captured, not dropped

## Post-Approval Actions

1. Save spec to `{artifacts.path}/specs/{feature-slug}/spec.md`
2. Update the issue in the issue tracker:
   - Add spec link to description
   - Change status to "In Progress" (or equivalent)
   - See [issue-tracker-adapters.md](issue-tracker-adapters.md) for exact commands
3. Show the spec path to the user
4. **Do not proceed to DESIGN or PLAN without explicit user approval**

## What Comes Next

- Gray areas detected → [discuss.md](discuss.md) before DESIGN
- Complex feature → [design.md](design.md)
- Simple/Medium → [plan.md](plan.md) directly
- Quick → skip entirely, go to [quick-mode.md](quick-mode.md)
