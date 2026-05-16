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

Describe the types, interfaces, and error cases that must exist after implementation.
Use the notation that matches the project's language (from `.specforge/architecture/conventions`).

### {ContextName}

```
Entity: {Entity}
  - id: string
  - createdAt: datetime
  - {field}: {type}

Repository/Store: {Entity}Repository
  - create(entity: {Entity}) → Result | Error
  - findById(id: string) → {Entity} | null | Error

Errors:
  - {Entity}NotFound — raised when entity does not exist
  - {Entity}Invalid  — raised when invariant is violated
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
