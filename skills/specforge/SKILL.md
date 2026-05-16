---
name: specforge
description: Spec-driven development framework. Full lifecycle from product discovery to review. Auto-sizes by complexity. Parallel execution via Worktrunk. Pluggable issue trackers (Linear default). SecondBrain or in-repo artifact storage. Use for: starting features, implementing from issues, quick fixes, mapping codebases, reviewing PRs, pausing/resuming work. Triggers on "discover features", "specify", "technical design", "tdd", "design", "plan tasks", "implement", "execute", "open PR", "review PR", "quick fix", "pause work", "resume work", "map codebase", "init project".
---

# SpecForge

Spec-driven development framework. Plan with precision. Execute in parallel. Learn continuously.

```
┌──────────┐  ┌─────────┐  ┌─────┐  ┌────────┐  ┌─────────┐  ┌────┐  ┌────┐
│ DISCOVER │→ │ SPECIFY │→ │ TDD │→ │ DESIGN │→ │  PLAN   │→ │ EX │→ │ PR │→ REVIEW
└──────────┘  └─────────┘  └─────┘  └────────┘  └─────────┘  └────┘  └────┘
   optional      always    optional   optional     optional    always  always*

* PR + REVIEW skipped in quick mode
```

## Project Config — Load First

Before any phase, check for `.specforge/project.yaml` in the project root. If it exists, load it. It defines where artifacts are saved, which issue tracker to use, and which execution backend to use.

If `.specforge/project.yaml` doesn't exist: run [project-init.md](references/project-init.md) first.

Key fields to extract:
- `artifacts.path` — where to save specs, plans, tasks
- `issue_tracker.type` — linear | github-issues | jira | none
- `execution.backend` — worktrunk | git-worktree | task-tool
- `execution.platform` — windows | unix
- `project.language` — language for all output (default: `pt-BR`)

**Language rule:** After loading `project.yaml`, apply `project.language` to ALL output for the rest of the session — every message to the user, every generated artifact (spec, plan, STATE.md entries, PR descriptions, review comments). If `project.language` is not set, default to `pt-BR`.

Always load `.specforge/STATE.md` when it exists — it holds decisions, blockers, and lessons that affect every phase.

## Auto-Sizing

**Assess complexity before starting any feature.** The scope determines the pipeline, not a fixed sequence.

| Complexity | Signals | Pipeline |
|---|---|---|
| **Quick** | ≤3 files, one-sentence change, no design decisions | [quick-mode](references/quick-mode.md) only |
| **Simple** | Single feature, clear scope, <5 obvious steps | SPECIFY → EXECUTE → PR |
| **Medium** | Multi-file, clear architecture, 5-10 tasks | SPECIFY → [TDD] → PLAN → EXECUTE → PR → REVIEW |
| **Complex** | Multi-context, ambiguous requirements, new patterns | DISCOVER → SPECIFY → discuss → [TDD] → DESIGN → PLAN → EXECUTE → PR → REVIEW |

**When to add TDD:** payments/auth/PII features, cross-team sign-off required, production systems with rollback plans, schema migrations. See [tdd.md](references/tdd.md) for full criteria.

**Safety valve:** Even when PLAN is skipped, EXECUTE must list atomic steps inline before starting. If that listing reveals >5 steps or complex dependencies, STOP and run [plan.md](references/plan.md) first.

## Phases

### DISCOVER (optional)
When to use: no clear feature defined yet, or starting from scratch.
Reference: [discover.md](references/discover.md)

Explores the codebase and issue tracker to propose prioritized features with value, complexity, and bounded context assessment.

### SPECIFY (always required)
Reference: [specify.md](references/specify.md)

Transforms a feature request or issue into a spec with traceable requirement IDs. Conducts a structured interview for complex features; generates directly for simple ones. Output includes problem statement, requirements (FEAT-NNN), acceptance criteria, bounded contexts, out of scope, risks.

Gray areas detected during SPECIFY trigger [discuss.md](references/discuss.md), which captures user decisions in `context.md` before proceeding.

### TDD — Technical Design Document (optional)
Reference: [tdd.md](references/tdd.md)

Produces a stakeholder-aligned technical design document before implementation begins. Required when sign-off is needed from product, security, or compliance; or when the feature involves payments, authentication, PII, schema migrations, or production rollback plans. Skipped for Quick and Simple features.

Adapts to project size (Small/Medium/Large) with mandatory and optional sections. After stakeholder approval, architectural decisions are copied to `.specforge/STATE.md`.

### DESIGN (optional — Large/Complex only)
Reference: [design.md](references/design.md)

Documents architectural decisions, component breakdown, interfaces, and data flow. Skipped when the change is straightforward. Required when new patterns are introduced or multiple bounded contexts interact.

### PLAN (optional — Medium/Complex)
Reference: [plan.md](references/plan.md)

Breaks the spec into atomic tasks with explicit dependencies, gate checks, and parallel execution graph. Each task is self-contained — a subagent can execute it with zero additional context.

### EXECUTE (always required)
Reference: [execute.md](references/execute.md)

Orchestrates parallel execution of tasks using the configured backend. Each task runs in isolation. Reads [execution-backends.md](references/execution-backends.md) for the exact commands based on `project.yaml`.

### PR (after execute)
Reference: [pr.md](references/pr.md)

Opens a pull request per task branch with evidence (API tests, screenshots), structured description, and issue tracker link. Updates issue status to "In Review".

### REVIEW (after PR)
Reference: [review.md](references/review.md)

Analyzes the PR from 4 perspectives: code quality, architecture, performance, security. Posts comments directly on the PR. Registers lessons in `.specforge/STATE.md` for the learning loop.

## Quick Mode
Reference: [quick-mode.md](references/quick-mode.md)

For ≤3 files, one-sentence scope. No spec, no plan. Implement → test → commit. Same quality principles, zero ceremony.

## State & Memory
Reference: [state-management.md](references/state-management.md)

`.specforge/STATE.md` is the project's persistent memory. Decisions (AD-NNN), blockers (B-NNN), lessons (L-NNN), deferred ideas, todos. Updated by REVIEW after each PR. Read by every phase before executing.

## Session Continuity
Reference: [session-handoff.md](references/session-handoff.md)

Pause work: creates `HANDOFF.md` with full context for resuming.
Resume work: loads HANDOFF.md + STATE.md + current spec/plan.

## Project Initialization
Reference: [project-init.md](references/project-init.md)

Bootstraps `.specforge/` structure: `project.yaml`, `PROJECT.md`, `ROADMAP.md`, `STATE.md`.

## Codebase Mapping (Brownfield)
Reference: [brownfield-mapping.md](references/brownfield-mapping.md)

Produces 7 structured documents from an existing codebase: STACK, ARCHITECTURE, CONVENTIONS, STRUCTURE, TESTING, INTEGRATIONS, CONCERNS. Run once before specifying features in an existing project.

## Knowledge Verification
See [coding-principles.md](references/coding-principles.md)

Before any technical decision, follow this chain:
1. Existing codebase + `.specforge/architecture/`
2. Project docs (README, inline docs)
3. Web search / official docs
4. Flag as uncertain — never fabricate

## Issue Tracker Commands
See [issue-tracker-adapters.md](references/issue-tracker-adapters.md) for the exact commands for the configured tracker type.

## Execution Backend Commands
See [execution-backends.md](references/execution-backends.md) for the exact commands for the configured backend.

## Context Budget
See [context-limits.md](references/context-limits.md)

Base load: PROJECT.md + STATE.md + current spec (~15k tokens).
Target: <40k total. Reserve 160k+ for work.
