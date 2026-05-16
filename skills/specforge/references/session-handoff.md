# Session Handoff — Pause & Resume

Preserve full context when pausing work. Resume without losing state.

## Pausing Work

Create `HANDOFF.md` in the current feature directory:
`{artifacts.path}/specs/{feature-slug}/HANDOFF.md`

Or at project root if work spans multiple features:
`.specforge/HANDOFF.md`

### HANDOFF.md Format

```markdown
# Handoff: {Feature or task name}

**Created:** {YYYY-MM-DD HH:MM}
**Phase:** Specify | Discuss | Design | Plan | Execute | PR | Review
**Issue:** {ISSUE-ID} — {url}

## Current State

{One paragraph: what is done, what is in progress, what is next.
 Be specific enough that a cold session can continue without context.}

## Completed Steps

- [x] {step description}
- [x] {step description}

## In Progress

- [ ] {step description} — {current status or blocker}

## Next Steps (in order)

1. {Exact next action}
2. {Following action}
3. {Following action}

## Open Decisions

{Any decisions that were not yet made and need to be picked up.}

## Files Modified So Far

- `{path}` — {what was changed}

## Important Context

{Anything non-obvious that the resuming session needs to know:
 design decisions made, user preferences expressed, constraints discovered.}

## Gate Status

Last gate run: {command}
Result: {PASS / FAIL / not run}
```

After writing HANDOFF.md:
1. Commit any uncommitted changes: `git add -A && git commit -m "wip({scope}): work in progress — handoff"`
2. Update STATE.md Todos section: add "[ ] Resume: {feature} from {phase}"
3. Report: "Work paused. HANDOFF.md created at {path}. Resume by loading this file."

## Resuming Work

1. Load `HANDOFF.md` — read completely
2. Load `.specforge/STATE.md` — check for new decisions, blockers, lessons since handoff
3. Load the current spec and plan for the feature
4. Check git status: any uncommitted changes? Understand their state
5. Report to user:
   ```
   Resuming: {feature name}
   Phase: {phase}
   Last gate: {status}
   Next step: {exact action from HANDOFF.md}
   ```
6. Proceed with the first item in "Next Steps"
7. Delete or archive HANDOFF.md once work resumes successfully
