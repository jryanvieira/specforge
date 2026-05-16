# Brownfield Mapping — Existing Codebase Analysis

Generate 7 structured documents from an existing codebase. Run once before specifying features in an existing project.

## When to Use

- Adopting SpecForge on an existing codebase
- Onboarding onto an unfamiliar project
- Before specifying a complex feature in a large codebase

## Output Documents

All saved to `.specforge/architecture/`:

1. `STACK.md` — languages, frameworks, libraries, versions
2. `ARCHITECTURE.md` — layers, patterns, component relationships
3. `CONVENTIONS.md` — naming, formatting, code patterns in use
4. `STRUCTURE.md` — directory layout and what lives where
5. `TESTING.md` — test types, coverage, gate commands
6. `INTEGRATIONS.md` — external services, APIs, databases
7. `CONCERNS.md` — tech debt, risks, fragile areas

## Exploration Protocol

For each document, follow the knowledge verification chain:
1. Read the codebase directly (files, directories, configs)
2. Read existing docs (README, inline comments)
3. Infer from patterns — do not fabricate
4. Flag uncertain areas explicitly

Explore in passes, not all at once. Use glob and grep to navigate efficiently.

---

## STACK.md

```markdown
# Stack

| Layer | Technology | Version | Notes |
|---|---|---|---|
| Language | {Go / TypeScript} | {version} | |
| Web framework | {Chi / Next.js} | {version} | |
| Database | {SQLite / PostgreSQL} | {version} | |
| Auth | {JWT / OAuth} | {library + version} | |
| Testing | {stdlib / Jest} | {version} | |
| Build | {Makefile / npm scripts} | — | |
| Container | {Docker / none} | — | |

## Key Libraries

| Library | Purpose | Import path |
|---|---|---|
| {library} | {what it does} | {import path} |
```

---

## ARCHITECTURE.md

```markdown
# Architecture

## Pattern

{DDD / Hexagonal / MVC / etc.} — {brief description of how it's applied}

## Layers

| Layer | Directory | Rule |
|---|---|---|
| Domain | {path} | {what can import} |
| Application | {path} | {what can import} |
| Infrastructure | {path} | {what can import} |
| Presentation | {path} | {what can import} |

## Bounded Contexts

| Context | Directory | Responsibility |
|---|---|---|
| {ContextName} | {path} | {what it owns} |

## Key Design Decisions

{Non-obvious decisions observed in the code, with evidence.}
```

---

## CONVENTIONS.md

```markdown
# Conventions

## Naming

- Files: {snake_case / camelCase / kebab-case}
- Types/Structs: {PascalCase}
- Functions: {camelCase / PascalCase}
- Error variables: {Err + PascalCase}

## Error Handling

{Pattern: wrapping, sentinel errors, custom types, etc.}

## ID Generation

{uuid.NewString() / cuid / etc.}

## Timestamps

{time.Now().UTC() / time.Now() / etc.}

## Money / Currency

{int64 centavos / decimal / etc.}

## Commit Format

{Conventional Commits / custom / etc.}

## Code Patterns Observed

{Patterns seen in the codebase that agents should replicate.
 Include code snippets for non-obvious patterns.}
```

---

## STRUCTURE.md

```markdown
# Directory Structure

{cmd/
  └── server/
      └── main.go       — entry point, composition root
internal/
  ├── domain/
  │   └── {context}/    — entities, repositories (interfaces), errors
  ├── application/
  │   └── {context}/    — use cases
  ├── infrastructure/
  │   └── repositories/ — repository implementations
  └── presentation/
      └── http/         — handlers, routes}

## Where to Put New Code

| Type | Location |
|---|---|
| New entity | `internal/domain/{context}/{entity}.go` |
| New use case | `internal/application/{context}/{usecase}.go` |
| New repository impl | `internal/infrastructure/repositories/{db}/{context}.go` |
| New handler | `internal/presentation/http/{context}_handler.go` |
| New migration | `migrations/{NNN}_{description}.sql` |
```

---

## TESTING.md

```markdown
# Testing

## Test Types

| Type | Location | Command |
|---|---|---|
| Unit (domain) | next to production files | `go test ./internal/domain/...` |
| Integration (infra) | `internal/infrastructure/...` | `go test ./internal/infrastructure/...` |
| End-to-end | `tests/` | `go test ./tests/...` |

## Coverage

{Observed coverage patterns — which areas have tests, which don't}

## Gate Commands

Full suite: `{command}`
Fast suite: `{command}`
Single context: `{command pattern}`

## Test Patterns

{How tests are structured — table-driven, subtests, helpers, mocking approach}
```

---

## INTEGRATIONS.md

```markdown
# Integrations

| Service | Type | How connected | Config |
|---|---|---|---|
| {service} | Database / API / Auth | {library / HTTP / MCP} | {env var / config file} |

## Environment Variables

| Variable | Purpose | Required |
|---|---|---|
| {VAR} | {what it does} | Yes / No |
```

---

## CONCERNS.md

```markdown
# Concerns

Tech debt, fragile areas, and risks. Read before planning features in these areas.

## Active Concerns

### C-{NNN}: {Short title}
**Category:** TechDebt | Bug | Security | Performance | TestGap | Fragile | Dependency
**Severity:** Low | Medium | High
**Area:** {file path or bounded context}
**Description:** {What the concern is, with evidence}
**Impact:** {What breaks or degrades if ignored}
**Suggested fix:** {How to address it}

---

## Resolved Concerns

(Moved here when addressed, with resolution notes)
```

---

## After Mapping

1. Save all 7 documents to `.specforge/architecture/`
2. Report summary:
   ```
   Brownfield mapping complete.
   
   Key findings:
   - Stack: {main technologies}
   - Pattern: {architecture pattern}
   - Bounded contexts: {N} ({list})
   - Active concerns: {N} ({severity breakdown})
   - Test coverage: {assessment}
   
   Review CONCERNS.md before starting feature work.
   ```
3. Proceed to [project-init.md](project-init.md) if PROJECT.md doesn't exist yet
