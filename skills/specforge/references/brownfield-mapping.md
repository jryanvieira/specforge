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
1. **Existing convention docs first** — CLAUDE.md, .cursorrules, .github/copilot-instructions.md, CONTRIBUTING.md, README.md. Import what's already documented.
2. Read the codebase directly (files, directories, configs, manifests)
3. Infer from patterns — do not fabricate
4. Flag uncertain areas explicitly with `[inferred]` or `[?]`

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

**Before analyzing code**, check for existing convention documentation:
- `CLAUDE.md` (project root) — import explicit rules directly
- `.cursorrules` / `.github/copilot-instructions.md` — editor rules
- `CONTRIBUTING.md` / `README.md` (dev section) — contribution guidelines

Import what's documented. Fill gaps with code analysis. Mark inferred items with `[inferred]`, missing with `[?]`.

```markdown
# Conventions

## Stack

| Component | Technology | Version |
|---|---|---|
| Language | {language} | {version} |
| Framework | {framework} | {version} |
| Test runner | {framework} | {version} |
| Linter / formatter | {tool} | {version} |

## Naming

- Files: {snake_case / camelCase / kebab-case / PascalCase}
- Types/Classes: {PascalCase / etc.}
- Functions/Methods: {camelCase / snake_case / etc.}
- Test files: {*.test.ext / *_test.ext / test_*.ext}
- Error types: {pattern observed}

## Error Handling

{Pattern: exceptions / result types / sentinel errors / custom error classes, etc.}

## ID Generation

{Library or pattern used — e.g., uuid v4, cuid2, nanoid, auto-increment}

## Timestamps

{Convention: UTC always / local / ISO string / Unix ms}

## Sensitive Values (money, scores, etc.)

{Convention for precision-sensitive types — e.g., integer cents, Decimal, BigDecimal}

## Test Command

Full suite: `{command}`
Single module: `{command pattern}`

## Commit Format

{Conventional Commits / Gitmoji / custom / etc.}

## Code Patterns Observed

{Non-obvious patterns seen in the codebase that agents should replicate.
 Include short snippets for patterns that aren't derivable from naming alone.}
```

---

## STRUCTURE.md

```markdown
# Directory Structure

{Actual directory tree from the project — use real paths, not placeholders}

## Where to Put New Code

| Type | Location |
|---|---|
| New entity / model | `{path}` |
| New service / use case | `{path}` |
| New persistence impl | `{path}` |
| New handler / controller | `{path}` |
| New migration | `{path}` |
| New test | `{path pattern}` |
```

---

## TESTING.md

```markdown
# Testing

## Test Types

| Type | Location | Command |
|---|---|---|
| Unit | `{path}` | `{command}` |
| Integration | `{path}` | `{command}` |
| End-to-end | `{path}` | `{command}` |

## Coverage

{Observed coverage patterns — which areas have tests, which don't}

## Gate Commands

Full suite: `{command}`
Fast suite: `{command}`
Single context: `{command pattern}`

## Test Patterns

{How tests are structured — table-driven, describe/it blocks, fixtures, mocking approach, assertion library}
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
