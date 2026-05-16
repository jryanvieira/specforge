# Plan — Atomic Task Breakdown

Break an approved spec into self-contained tasks ready for parallel execution.

## Pre-Work

1. Load the approved `spec.md`
2. Load `design.md` if it exists
3. Load `context.md` if it exists
4. Load `.specforge/architecture/` — task boundaries must respect layer rules
5. Load `.specforge/STATE.md` — check lessons before defining tasks

## When to Skip

Skip PLAN and go directly to EXECUTE when:
- Feature is Simple complexity (≤5 obvious steps, single developer, no parallel work)
- Only 1 file changes
- No dependencies between work units

In those cases, EXECUTE lists steps inline before starting. If that listing reveals >5 steps or shared files, come back here.

## Task Design Principles

**Atomic:** Each task has exactly one deliverable — one layer, one component, one interface.

**Self-contained:** A subagent can execute the task with zero additional context. The task definition includes everything needed.

**Independent when possible:** Tasks that don't share files can run in parallel. Maximize parallelism.

**Test co-located:** Tests are not separate tasks. The task that creates a component also creates its tests.

## Task Format

Each task follows this exact format:

```markdown
### TASK-N: {Descriptive Name}

**Branch:** {tracker-prefix}/{ISSUE-ID}-{short-name}
**Complexity:** {estimated hours or story points — optional}
**Can start:** Immediately | After TASK-X is merged
**Depends on:** None | TASK-X
**Unlocks:** None | TASK-Y, TASK-Z

**Requirements covered:** {PREFIX}-001, {PREFIX}-002

**Domain context:**
{What the subagent needs to know — relevant excerpt from architecture guides,
 existing patterns to follow, invariants to respect.
 Do NOT reference other tasks — the subagent only sees this task.}

**Files to create/modify:**
- `{path/to/file.{ext}}` — {what to do: create entity, add method, implement interface}
- `{path/to/test_file.{ext}}` — {what to test}

Use the file naming and directory conventions from `.specforge/architecture/conventions`.

**Expected contracts at completion:**
{Exact interface/contract — in the project's language or pseudocode — that must exist when done.
 Copied or derived from spec.md interfaces section.}

**Tests that must pass:**
- `{test-command}` — {scenario description}
- Scenario: Given {context}, when {action}, then {outcome}
- Scenario: Given {edge case}, when {action}, then {error type}

**Gate:**
```
{test-command from .specforge/architecture/conventions}
```
All tests pass. Build succeeds. No linter errors.

**Done when:**
{Objective, binary description. "The Create{Entity} use case exists, accepts {EntityRequest}, validates all required fields, persists via {Entity}Repository, and returns {Entity}Invalid for invalid inputs."}
```

## Dependency Graph

After defining all tasks, draw the execution graph:

```markdown
## Execution Graph

```
TASK-1 (domain entities)
  └── TASK-2 (repository interface)    ← depends on TASK-1
  └── TASK-3 (use cases)               ← depends on TASK-1
        └── TASK-4 (HTTP handlers)     ← depends on TASK-3
              └── TASK-5 (integration) ← depends on TASK-4

Parallel groups:
- Start now:  TASK-1
- After 1:    TASK-2, TASK-3 (parallel)
- After 3:    TASK-4
- After 4:    TASK-5
```
```

## Parallelism Rules

- Tasks are parallel if they touch different files
- Tasks that both write to the same file must be sequential
- Follow the dependency order defined in `.specforge/architecture/` (e.g., domain before application, data layer before presentation)

## Output

Save plan to: `{artifacts.path}/plans/{YYYY-MM-DD}-{feature-slug}.md`

For issue trackers that support sub-issues: create one sub-issue per task after plan approval. See [issue-tracker-adapters.md](issue-tracker-adapters.md).

## Validation Checklist

- [ ] Every task has a binary Done When criterion
- [ ] Every task has a Gate command that verifies completion
- [ ] Parallel tasks touch different files (no shared writes)
- [ ] Each task has enough context for a subagent to work alone
- [ ] Dependency graph has no cycles
- [ ] All P1 requirements from spec are covered by at least one task
- [ ] Tests are co-located with implementation (not separate tasks)
- [ ] Branch names follow convention from `project.yaml` or architecture guide

## After Approval

1. Show execution graph to user
2. Create sub-issues in issue tracker if supported (see [issue-tracker-adapters.md](issue-tracker-adapters.md))
3. Save plan to artifacts path
4. Proceed to [execute.md](execute.md)
