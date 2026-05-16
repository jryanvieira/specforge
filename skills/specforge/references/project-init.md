# Project Init — Bootstrap SpecForge for a Project

Set up `.specforge/` structure for a new or existing project.

## When to Use

- Starting a brand new project
- Adopting SpecForge in an existing codebase
- After cloning a repo that uses SpecForge (if `.specforge/` is gitignored)

## Steps

### 1. Gather Project Information

Ask the user (one question at a time if not already known):

1. Project name and short description
2. Language for generated artifacts and communication — default is `pt-BR`. Examples: `pt-BR`, `en-US`, `es-ES`, `fr-FR`, `de-DE`. From this point on, use the chosen language for all output.
3. Tech stack (language, framework, database) — skip if detectable from manifest files
4. Where should specs/plans be saved? (path, or "in-repo" for `.specs/`)
5. Issue tracker: Linear / GitHub Issues / Jira / none?
   - If Linear: workspace slug?
6. Execution backend: Worktrunk (Windows) / git worktree / Task tool?

### 2. Convention Auto-Detection

Before asking the user anything about conventions, scan the project for existing documentation. Do this in parallel — all reads are independent.

**Sources to scan (check all that exist):**

| File | What to extract |
|---|---|
| `CLAUDE.md` (project root) | Explicit coding rules, patterns, constraints |
| `.cursorrules` | Editor conventions |
| `.github/copilot-instructions.md` | Editor conventions |
| `CONTRIBUTING.md` | Contribution guidelines, naming rules |
| `README.md` | Development setup section, coding guidelines |

**Stack detection from manifests (check all that exist):**

| File | Signals |
|---|---|
| `package.json` | JS/TS stack, test runner (`jest`, `vitest`, `mocha`), lint config |
| `go.mod` | Go version, key dependencies |
| `requirements.txt` / `pyproject.toml` | Python stack and test framework |
| `Gemfile` | Ruby stack |
| `pom.xml` / `build.gradle` | Java/Kotlin stack |
| `*.sln` / `*.csproj` | .NET stack |
| `Cargo.toml` | Rust stack |

**Code pattern sampling (read 3–5 files):**
- One entity/model file — infer naming conventions, type patterns
- One test file — infer test framework, assertion style, file naming
- One handler/controller/route file — infer HTTP layer patterns
- Entry point (`main.go`, `index.ts`, `app.py`, `server.js`, etc.) — infer wiring pattern

**Generate draft `conventions.md`:**

Combine all findings into a draft using the CONVENTIONS.md format from [brownfield-mapping.md](brownfield-mapping.md). Mark each item with its confidence:
- Content found in docs → use as-is
- Inferred from code → mark with `[inferred]`
- Not found anywhere → mark with `[?]`

**Present to the user:**

Show the draft and ask:
> "Encontrei estas convenções no projeto. Confirma, corrige ou preenche os campos `[?]`."

Wait for the user to confirm or adjust before saving.

Save approved content to `.specforge/architecture/conventions.md`.

---

### 3. Create Directory Structure

```
.specforge/
├── project.yaml
├── STATE.md
├── PROJECT.md
├── ROADMAP.md
└── architecture/       (empty — populated by user or brownfield mapping)
```

### 4. Write project.yaml

Use `templates/project.yaml` as base, filling in answers from step 1.

### 5. Write PROJECT.md

```markdown
# {Project Name}

## Vision

{One sentence: what this project does and for whom.}

## Goals

- {Goal 1}
- {Goal 2}

## Non-Goals

- {What this project explicitly does not try to do}

## Stack

| Component | Technology |
|---|---|
| Language | {Go / TypeScript / etc.} |
| Framework | {Chi / Next.js / etc.} |
| Database | {SQLite / PostgreSQL / etc.} |
| Auth | {JWT / OAuth / etc.} |

## Constraints

- {Technical, compliance, or operational constraint}
- {Constraint}
```

### 6. Write ROADMAP.md

```markdown
# Roadmap

## Current Milestone: {Milestone Name}

**Goal:** {What "done" looks like for this milestone}
**Target:** {YYYY-MM or "no deadline"}

### Planned Features

| Feature | Complexity | Status | Issue |
|---|---|---|---|
| {feature name} | Simple/Medium/Complex | Planned/In Progress/Done | {ISSUE-ID} |

## Backlog

| Feature | Complexity | Notes |
|---|---|---|
| {feature name} | {complexity} | {why deferred} |
```

### 7. Write STATE.md

Use `templates/state-template.md` — creates an empty STATE.md with all sections initialized.

### 8. Full Architecture Mapping (existing codebases only)

If `conventions.md` from step 2 revealed a complex codebase with multiple layers and patterns, run [brownfield-mapping.md](brownfield-mapping.md) to generate the remaining 6 architecture documents (STACK, ARCHITECTURE, STRUCTURE, TESTING, INTEGRATIONS, CONCERNS).

For new projects: `conventions.md` from step 2 is sufficient. The remaining documents grow organically as the codebase is built.

## After Init

1. Commit `.specforge/` to the repository
2. Report:
   ```
   SpecForge initialized.
   
   Config: .specforge/project.yaml
   Artifacts: {artifacts.path}
   Issue tracker: {type}
   Execution: {backend}
   
   Next: run /specforge to start your first feature, or
         run /specforge map codebase if this is an existing project.
   ```
