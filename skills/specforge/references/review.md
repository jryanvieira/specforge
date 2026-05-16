# Review — Multi-Perspective PR Analysis

Analyze a PR from 4 independent perspectives. Post comments. Register lessons.

## Pre-Work

1. Load `.specforge/architecture/` files — validate against project standards
2. Load `.specforge/STATE.md` — especially the Lessons section (L-NNN entries)
3. Load the spec for this feature: `{artifacts.path}/specs/{feature-slug}/spec.md`
4. Fetch the PR diff:
   ```bash
   gh pr diff {PR-NUMBER}
   ```
5. Load PR metadata:
   ```bash
   gh pr view {PR-NUMBER} --json title,body,files,commits
   ```

## 4 Perspectives

Run each perspective as an independent analysis pass. Do not mix concerns.

---

### Perspective 1: Code Quality

Focus: idiomatism, clarity, correctness for the project's stack.

Check for:
- Naming conventions (follows project conventions from `.specforge/architecture/`)
- Functions doing one thing
- Error handling completeness (every error path handled, not swallowed)
- No dead code introduced
- Proper use of context.Context propagation
- No magic numbers or unexplained constants
- Test coverage for happy path AND error paths
- Test assertions are meaningful (not just "no error")

---

### Perspective 2: Architecture & Domain

Focus: bounded context boundaries, layer rules, domain model integrity.

Check for:
- Domain layer imports only stdlib + allowed dependencies (no infra, no presentation)
- Business logic lives in domain or application, not in handlers or repositories
- Repository is an interface in domain, not a concrete type
- Value objects are immutable
- Money values are always `int64` centavos — never `float64`
- Timestamps are `time.Time` with `.UTC()`
- Date strings are `"YYYY-MM-DD"` format
- UUIDs generated with `uuid.NewString()`
- Domain errors are typed (`var ErrXxx = errors.New(...)` in context's `errors.go`)
- No business rules duplicated across bounded contexts
- Aggregate invariants are enforced in the `New` constructor, not in handlers

---

### Perspective 3: Performance

Focus: efficiency, resource usage, query patterns.

Check for:
- N+1 queries (loop calling repository per iteration)
- Missing database indexes for new query patterns
- Unbounded queries (SELECT without LIMIT where cardinality can grow)
- Resource leaks (unclosed rows, unclosed files, goroutine leaks)
- Unnecessary allocations in hot paths
- Missing `context.Context` cancellation checks in loops
- Large payloads returned when only a subset is needed

---

### Perspective 4: Security

Focus: authorization, data integrity, information disclosure.

Check for:
- Missing authentication/authorization on new endpoints
- Multi-tenant data isolation (user can only access their own data)
- SQL injection risk (raw string interpolation in queries)
- Sensitive data in logs or error messages returned to client
- JWT validation on protected routes
- Input validation at API boundary (not trusting client-supplied IDs)
- Passwords or secrets never stored in plain text
- Error messages that reveal internal structure to the client

---

## Comment Format

Post one comment per finding:

```
[{Perspective}] {File}:{Line}

**Problem:** {What is wrong and why it matters}
**Suggestion:**
```{language}
{corrected code snippet — concrete, not abstract}
```
```

Group minor style notes into a single comment to avoid noise.

## Severity Levels

- **Blocking:** Must fix before merge (security issue, data corruption risk, broken test, violated architecture rule)
- **Important:** Should fix (missing test coverage, performance issue, unclear naming)
- **Note:** Optional improvement (style preference, minor refactor opportunity)

Mark blocking issues clearly. Approve only when all blocking issues are resolved.

## Learning Loop (Mandatory)

After the review, register every blocking or important finding in `.specforge/STATE.md` as a lesson:

```markdown
### L-{NNN}: {Short title}

**Date:** {YYYY-MM-DD}
**Category:** CodeQuality | Architecture | Performance | Security
**Error:** {What was done wrong}
**Why it's wrong:** {The principle or invariant violated}
**Correct approach:**
```{language}
{correct code snippet}
```
**Bounded context / area:** {where this applies}
```

Increment the L-NNN counter from the last entry in STATE.md.

This is non-negotiable. Every lesson registered here is read by all future agents before they write code, preventing the same mistake from recurring.

## Approval

After all perspectives are complete:
- If blocking issues found: post summary comment, request changes (`gh pr review {NUMBER} --request-changes --body "..."`)
- If only important/notes: post summary, approve (`gh pr review {NUMBER} --approve --body "..."`)
- If clean: approve with brief comment

Report to user:
```
Review complete for PR #{NUMBER}.
Blocking issues: {N}
Important: {N}
Notes: {N}
Lessons registered in STATE.md: {L-NNN, L-NNN}
```
