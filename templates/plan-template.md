# Plan: {Feature Title}

**Issue:** {ISSUE-ID}
**Spec:** {path to spec.md}
**Date:** {YYYY-MM-DD}
**Status:** Draft

---

## Execution Graph

```
TASK-1 ({short name})
  ├── TASK-2 ({short name})    ← depends on TASK-1
  └── TASK-3 ({short name})    ← depends on TASK-1, parallel with TASK-2
        └── TASK-4 ({short name}) ← depends on TASK-2 and TASK-3

Parallel groups:
- Start now:  TASK-1
- After 1:    TASK-2, TASK-3 (parallel)
- After 2+3:  TASK-4
```

---

## Tasks

### TASK-1: {Descriptive Name}

**Branch:** task/{ISSUE-ID}-{short-name}
**Can start:** Immediately
**Depends on:** None
**Unlocks:** TASK-2, TASK-3
**Requirements covered:** {PREFIX}-001, {PREFIX}-002

**Domain context:**
{What the subagent needs to know. Relevant architecture rules, patterns to follow,
 invariants to respect. Self-contained — no references to other tasks.}

**Files to create/modify:**
- `{domain-layer}/{context}/{entity}.{ext}` — {what to do: create entity, add method}
- `{domain-layer}/{context}/{entity}_test.{ext}` — {what to test}
- `{domain-layer}/{context}/errors.{ext}` — {error/exception types for this context}

**Expected contracts at completion:**
```
Entity: {Entity}
  - id: string
  - createdAt: datetime

Constructor: new{Entity}(params) → {Entity} | Error
Validates: {invariant 1}, {invariant 2}

Errors: {Entity}Invalid, {Entity}NotFound
```

**Tests that must pass:**
- {test-command from project conventions} — {scenario description}
- Given valid input, new{Entity} returns entity without error
- Given invalid {field}, new{Entity} returns {Entity}Invalid error

**Gate:**
```
{test-command from .specforge/architecture/conventions — e.g., npm test, pytest, go test ./...}
```

**Done when:**
{Entity} type exists with constructor that validates all required fields. Error types defined. All unit tests pass.

---

### TASK-2: {Descriptive Name}

**Branch:** task/{ISSUE-ID}-{short-name-2}
**Can start:** After TASK-1 is merged
**Depends on:** TASK-1
**Unlocks:** TASK-4
**Requirements covered:** {PREFIX}-001

**Domain context:**
{Context for this task...}

**Files to create/modify:**
- `{domain-layer}/{context}/repository.{ext}` — repository interface/protocol
- `{data-layer}/{context}.{ext}` — persistence implementation
- `{data-layer}/{context}_test.{ext}` — integration tests

**Expected contracts at completion:**
```
Interface: {Entity}Repository
  - create(entity: {Entity}) → Result | Error
  - findById(id: string) → {Entity} | null | Error
  - listByUser(userId: string) → [{Entity}] | Error
```

**Tests that must pass:**
- {test-command} — create persists entity and can be retrieved by findById
- findById returns {Entity}NotFound for unknown id
- listByUser returns only entities belonging to the given user

**Gate:**
```
{test-command for data layer}
```

**Done when:**
{Entity}Repository interface/protocol defined. Persistence implementation complete. All integration tests pass against a real database.

---

### TASK-3: {Descriptive Name}

**Branch:** task/{ISSUE-ID}-{short-name-3}
**Can start:** After TASK-1 is merged
**Depends on:** TASK-1
**Unlocks:** TASK-4
**Requirements covered:** {PREFIX}-001, {PREFIX}-003

**Domain context:**
{Context for this task...}

**Files to create/modify:**
- `{application-layer}/{context}/create_{entity}.{ext}` — use case / service
- `{application-layer}/{context}/create_{entity}_test.{ext}` — use case tests

**Expected contracts at completion:**
```
UseCase/Service: Create{Entity}
  Input:  { userId: string, {other fields} }
  Output: { id: string, {other fields} } | Error

Orchestrates: validates input → calls repository → returns result
```

**Tests that must pass:**
- {test-command} — execute with valid input creates entity and returns response
- execute with invalid {field} returns domain error
- execute propagates repository errors

**Gate:**
```
{test-command for application layer}
```

**Done when:**
Create{Entity} use case/service implemented. Tests use a stub/mock repository. All tests pass.

---

### TASK-4: {Descriptive Name}

**Branch:** task/{ISSUE-ID}-{short-name-4}
**Can start:** After TASK-2 and TASK-3 are merged
**Depends on:** TASK-2, TASK-3
**Unlocks:** None
**Requirements covered:** {PREFIX}-001, {PREFIX}-002, {PREFIX}-003

**Domain context:**
{Context for this task...}

**Files to create/modify:**
- `{presentation-layer}/{context}_handler.{ext}` — HTTP handler / controller / route
- `{presentation-layer}/{context}_handler_test.{ext}` — handler tests
- `{entry-point}` — wire use case and handler

**Expected contracts at completion:**
```
POST /api/v1/{resource}
Authorization: Bearer {token}
Body: { {fields} }

201: { "id": "...", {fields} }
400: { "error": "{entity}: invalid" }
401: (missing/invalid token)
```

**Tests that must pass:**
- {test-command} — POST with valid body returns 201 with created entity
- POST with invalid body returns 400
- POST without auth returns 401

**Gate:**
```
{full test-command — runs all tests}
```

**Done when:**
Handler registered on router. End-to-end: POST creates entity and returns 201. All tests pass.
