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
- `internal/domain/{context}/{entity}.go` — create entity with New and Reconstruct constructors
- `internal/domain/{context}/{entity}_test.go` — unit tests for validation
- `internal/domain/{context}/errors.go` — domain error types

**Expected interfaces at completion:**
```go
type {Entity} struct {
    ID        string
    UserID    string
    Amount    int64     // centavos
    CreatedAt time.Time
}

func New{Entity}(userID string, amount int64) (*{Entity}, error)
func Reconstruct{Entity}(id, userID string, amount int64, createdAt time.Time) *{Entity}

var (
    Err{Entity}Invalid  = errors.New("{entity}: invalid")
    Err{Entity}NotFound = errors.New("{entity}: not found")
)
```

**Tests that must pass:**
- `go test ./internal/domain/{context}/...`
- Given valid input, New{Entity} returns entity without error
- Given zero amount, New{Entity} returns Err{Entity}Invalid
- Given negative amount, New{Entity} returns Err{Entity}Invalid

**Gate:**
```
cd backend && go test ./internal/domain/{context}/...
```

**Done when:**
{Entity} struct exists with New and Reconstruct constructors. New validates all fields.
Domain error types are defined. All unit tests pass.

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
- `internal/domain/{context}/repository.go` — repository interface
- `internal/infrastructure/repositories/sqlite/{context}.go` — SQLite implementation
- `internal/infrastructure/repositories/sqlite/{context}_test.go` — integration tests

**Expected interfaces at completion:**
```go
type {Entity}Repository interface {
    Create(ctx context.Context, e *{Entity}) error
    FindByID(ctx context.Context, id string) (*{Entity}, error)
    ListByUser(ctx context.Context, userID string) ([]*{Entity}, error)
}
```

**Tests that must pass:**
- `go test ./internal/infrastructure/repositories/sqlite/...`
- Create persists entity and can be retrieved by FindByID
- FindByID returns Err{Entity}NotFound for unknown ID
- ListByUser returns only entities belonging to the given user

**Gate:**
```
cd backend && go test ./internal/infrastructure/repositories/sqlite/...
```

**Done when:**
{Entity}Repository interface defined in domain. SQLite implementation complete.
All integration tests pass against a real (in-memory) SQLite database.

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
- `internal/application/{context}/create_{entity}.go` — use case
- `internal/application/{context}/create_{entity}_test.go` — use case tests

**Expected interfaces at completion:**
```go
type Create{Entity}Request struct {
    UserID string
    Amount int64
}

type Create{Entity}Response struct {
    ID        string
    Amount    int64
    CreatedAt time.Time
}

type Create{Entity}UseCase struct {
    repo domain.{Entity}Repository
}

func (uc *Create{Entity}UseCase) Execute(ctx context.Context, req Create{Entity}Request) (*Create{Entity}Response, error)
```

**Tests that must pass:**
- `go test ./internal/application/{context}/...`
- Execute with valid request creates entity and returns response
- Execute with invalid amount returns domain error
- Execute propagates repository errors

**Gate:**
```
cd backend && go test ./internal/application/{context}/...
```

**Done when:**
Create{Entity}UseCase implemented with Execute method. Tests use a mock/stub repository.
All use case tests pass.

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
- `internal/presentation/http/{context}_handler.go` — HTTP handler
- `internal/presentation/http/{context}_handler_test.go` — handler tests
- `cmd/server/main.go` — wire use case and handler

**Expected interfaces at completion:**
```
POST /api/v1/{resource}
Authorization: Bearer {token}
Body: { "amount": 1000 }
Response 201: { "id": "...", "amount": 1000, "created_at": "..." }
Response 400: { "error": "{entity}: invalid" }
Response 401: (missing/invalid token)
```

**Tests that must pass:**
- `go test ./internal/presentation/http/...`
- POST with valid body returns 201 with created entity
- POST with invalid body returns 400
- POST without auth returns 401

**Gate:**
```
cd backend && go test ./...
```

**Done when:**
HTTP handler registered on router. End-to-end: POST creates entity and returns 201.
All tests pass including handler tests.
