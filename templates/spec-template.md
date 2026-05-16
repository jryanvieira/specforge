# Spec: {Feature Title}

**Issue:** {ISSUE-ID} | {url}
**Date:** {YYYY-MM-DD}
**Status:** Draft
**Complexity:** Quick | Simple | Medium | Complex
**Bounded Contexts:** {list}

---

## Problem Statement

{One to three sentences. What user problem does this solve? Avoid implementation details.}

---

## Requirements

### Functional

- **{PREFIX}-001** [P1] {Requirement description}
  - Acceptance: Given {context}, when {action}, then {outcome}

- **{PREFIX}-002** [P1] {Requirement description}
  - Acceptance: Given {context}, when {action}, then {outcome}

- **{PREFIX}-003** [P2] {Requirement description}
  - Acceptance: Given {context}, when {action}, then {outcome}

### Non-Functional

- **{PREFIX}-010** [P2] {Performance / security / reliability requirement}
  - Acceptance: {measurable criterion}

---

## Bounded Contexts

| Context | Impact | Change Type |
|---|---|---|
| {ContextName} | {what changes} | Addition \| Modification \| Read-only |

---

## Interfaces & Contracts

### {ContextName}

```go
type {Entity} struct {
    ID        string
    CreatedAt time.Time
    // ...
}

type {Repository} interface {
    Create(ctx context.Context, e *{Entity}) error
    FindByID(ctx context.Context, id string) (*{Entity}, error)
    ListByUser(ctx context.Context, userID string) ([]*{Entity}, error)
}

// Domain errors
var (
    Err{Entity}NotFound = errors.New("{entity}: not found")
    Err{Entity}Invalid  = errors.New("{entity}: invalid")
)
```

---

## Out of Scope

- {Thing explicitly excluded from this spec}
- {Related feature that will be a separate spec}

---

## Risks

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| {risk description} | Low \| Med \| High | Low \| Med \| High | {mitigation} |

---

## Deferred Ideas

{Good ideas captured during spec interview that are out of scope for now.
 These will be added to STATE.md as deferred ideas.}

- {idea}
