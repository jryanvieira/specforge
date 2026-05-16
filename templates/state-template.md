# Project State

_Persistent memory for this project. Updated by REVIEW after each PR. Read by all agents before executing._

---

## Decisions

_Architecture and product decisions. Use AD-NNN format, incrementing from AD-001._

<!-- Example:
### AD-001: SQLite as primary database

**Date:** 2026-01-15
**Context:** Need persistent storage for a single-user local app
**Decision:** Use SQLite via modernc.org/sqlite (pure Go, no CGO)
**Rationale:** No server setup, zero ops, sufficient for expected data volume
**Consequences:** Cannot use PostgreSQL-specific features; enables easy backup via file copy
-->

---

## Blockers

_Active blockers. Use B-NNN format, incrementing from B-001._

<!-- Example:
### B-001: Missing SQLite migration tool

**Status:** Resolved
**Opened:** 2026-01-20
**Resolved:** 2026-01-22
**Description:** No migration runner in place, blocking all schema changes
**Impact:** TASK-2 (repository), TASK-3 (use cases) cannot start
**Resolution:** Implemented raw SQL migrations applied at startup in main.go
-->

---

## Lessons

_Registered by REVIEW after each PR. Read by all agents before writing code. Use L-NNN format, incrementing from L-001._

<!-- Example:
### L-001: Money must always be int64 centavos

**Date:** 2026-01-25
**Category:** Architecture
**Error:** Used float64 for expense amount, causing precision loss in calculations
**Why it's wrong:** Floating-point arithmetic introduces rounding errors in financial calculations
**Correct approach:**
```go
// Wrong
Amount float64

// Correct
Amount int64 // always centavos: R$ 10,50 = 1050
```
**Area:** All bounded contexts with financial values
-->

---

## Deferred Ideas

_Good ideas captured during spec/discuss/review that are out of scope for now._

<!-- Example:
- [ ] 2026-01-20 Add bulk import from CSV — captured during goal spec, deferred to backlog
-->

---

## Todos

_In-progress and pending items._

<!-- Example:
- [ ] Write integration tests for expense repository
- [x] 2026-01-22 Set up CI pipeline
-->

---

## Preferences

_Model behavior guidance accumulated over sessions._

<!-- Example:
- Prefer table-driven tests over individual test functions
- Always show the execution graph before starting EXECUTE phase
-->
