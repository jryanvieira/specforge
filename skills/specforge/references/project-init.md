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
3. Tech stack (language, framework, database)
4. Where should specs/plans be saved? (path, or "in-repo" for `.specs/`)
5. Issue tracker: Linear / GitHub Issues / Jira / none?
   - If Linear: workspace slug?
6. Execution backend: Worktrunk (Windows) / git worktree / Task tool?
7. Are there existing architecture guides? (DDD patterns, layer rules, conventions?)

### 2. Create Directory Structure

```
.specforge/
├── project.yaml
├── STATE.md
├── PROJECT.md
├── ROADMAP.md
└── architecture/       (empty — populated by user or brownfield mapping)
```

### 3. Write project.yaml

Use `templates/project.yaml` as base, filling in answers from step 1.

### 4. Write PROJECT.md

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

### 5. Write ROADMAP.md

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

### 6. Write STATE.md

Use `templates/state-template.md` — creates an empty STATE.md with all sections initialized.

### 7. Architecture Guides (for existing codebases)

If the project has established patterns that agents must follow:

**Option A — Manual:** Have the user describe or paste existing architecture guides. Save to `.specforge/architecture/`.

**Option B — Brownfield mapping:** Run [brownfield-mapping.md](brownfield-mapping.md) to generate architecture docs from the existing codebase automatically.

For new projects: create `.specforge/architecture/conventions.md` with project conventions as they are established.

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
