# Execute — Parallel Orchestration

Orchestrate parallel task execution using the configured backend.

## Pre-Work

1. Load `.specforge/project.yaml` — extract `execution.backend` and `execution.platform`
2. Load the approved plan (tasks.md or plan file)
3. Load [execution-backends.md](execution-backends.md) for exact commands
4. Confirm repo is clean: no uncommitted changes, on main/trunk branch
5. Confirm all upstream dependencies (packages, migrations) are merged

## Execution Algorithm

```
1. Identify tasks with "Can start: Immediately" → launch all in parallel
2. For each task ready to start:
   a. Create isolated worktree/branch using backend commands
   b. Spawn subagent with the standard instruction block (see below)
   c. Update issue tracker: task → "In Progress"
3. When a task reports completion:
   a. Run Gate command — verify it passes
   b. If Gate fails: report to user, do NOT merge, await instruction
   c. If Gate passes: open PR for this branch (delegate to pr.md)
   d. Identify tasks this completion unlocks
   e. Launch newly unblocked tasks
   f. Update issue tracker: task → "In Review"
4. Repeat until all tasks are "In Review"
5. Report all PR URLs to user
```

**CRITICAL:** No merge to main without human approval. Every task becomes a PR. The user validates code before any merge.

## Subagent Instruction Block

Each subagent receives this instruction, adapted per task:

```
You are implementing TASK-N of the {feature-name} feature.

MANDATORY pre-work — read these files before writing any code:
- .specforge/architecture/{domain}-expert.md (if exists)
- .specforge/architecture/conventions.md (if exists)
- .specforge/STATE.md (lessons section — avoid past mistakes)

Your task definition:
{FULL CONTENT OF THE TASK SECTION FROM THE PLAN}

Context files to load on demand:
- {artifacts.path}/specs/{feature-slug}/spec.md (requirements reference)
- {artifacts.path}/specs/{feature-slug}/design.md (if exists — architecture reference)
- {artifacts.path}/specs/{feature-slug}/context.md (if exists — user decisions)

Implementation rules:
1. Read the Gate command in your task — that is your success criterion
2. Follow the coding principles in .specforge/architecture/conventions.md
3. Test co-location: write tests in the same commit as the implementation
4. Surgical changes: touch only files listed in your task
5. Atomic commit: one commit per task, message format: {commit-type}({scope}): {description}

When done:
1. Run the Gate command — confirm it passes
2. Make a single commit: {commit-prefix}({issue-id}): {task description}
3. Do NOT merge to main
4. Report: "TASK-N complete. Gate: PASS. Files changed: [list]."
```

## Task Status Updates

Update issue tracker at each state change. See [issue-tracker-adapters.md](issue-tracker-adapters.md) for exact commands.

| Event | Status |
|---|---|
| Task starts | In Progress |
| Gate passes, PR opened | In Review |
| PR merged (by user) | Done |

## Failure Handling

If a subagent fails or Gate does not pass:
1. Do NOT merge or unblock dependent tasks
2. Report to user with the exact error output
3. Await instruction — options: retry, fix manually, skip task
4. Do NOT cancel other parallel tasks that are running

If a dependency task fails and it blocks later tasks: pause the dependent tasks, report the blocked state clearly.

## Simple Features (No Formal Plan)

When PLAN was skipped (Simple complexity), EXECUTE lists atomic steps inline first:

```
Steps I will execute:
1. {step description} — touches: {file}
2. {step description} — touches: {file}
3. ...

Gate: {command}
```

If listing reveals >5 steps or shared files: STOP. Run [plan.md](plan.md) first.

If ≤5 steps: proceed without worktree isolation (implement directly on feature branch).

## Completion Condition

EXECUTE is complete when:
- All tasks have Gate passing
- All tasks have a PR open with link recorded in issue tracker
- User has been notified with all PR URLs
- Issue tracker reflects "In Review" for all tasks

The executor does NOT merge. Ever.
