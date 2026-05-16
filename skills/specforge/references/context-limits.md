# Context Limits — Token Budget Management

Keep the context lean so there's room to think, reason, and produce output.

## Budget Targets

| Zone | Token range | Action |
|---|---|---|
| Green | < 40k | Normal operation |
| Yellow | 40k – 60k | Warn user, unload non-essential docs |
| Red | > 60k | Stop loading new files, summarize completed phases |

**Reserve:** 160k+ tokens for work, reasoning, and output generation.

## Base Load (~15k tokens)

Always load at session start:

- `.specforge/project.yaml` (~1k)
- `.specforge/STATE.md` (~3-5k, grows over time)
- `.specforge/PROJECT.md` (~2k)
- Current spec file for the active feature (~3-5k)

## On-Demand Load

Load only when the phase needs it:

| File | Load when |
|---|---|
| `.specforge/ROADMAP.md` | DISCOVER phase |
| `.specforge/architecture/*.md` | SPECIFY, DESIGN, EXECUTE phases |
| `design.md` | PLAN, EXECUTE phases |
| `context.md` | DESIGN, PLAN, EXECUTE phases |
| `plan.md` / `tasks.md` | EXECUTE phase |
| PR diff | REVIEW phase |

## Never Load Simultaneously

- Multiple feature specs (load only the active one)
- Multiple architecture docs if total exceeds yellow zone (summarize instead)
- Full PR diff if > 500 lines (load by file, not all at once)

## Yellow Zone Actions

When approaching 40k tokens:
1. Unload completed phase reference files
2. Summarize completed tasks into a one-paragraph status instead of keeping full task definitions
3. Warn: "Context is at ~{N}k tokens. Unloaded {files} to stay in budget."

## Red Zone Actions

When above 60k tokens:
1. STOP loading new files
2. Summarize the current phase completion status
3. Ask user if they want to continue in a new session or proceed with reduced context
4. Create/update HANDOFF.md before ending if they choose new session

## File Size Targets

Keep individual files within these ranges to avoid context bloat:

| File | Target size |
|---|---|
| STATE.md | < 5k tokens (archive old lessons after 20 entries) |
| spec.md | < 3k tokens per feature |
| design.md | < 4k tokens per feature |
| plan.md | < 5k tokens (large features may exceed) |
| architecture/*.md | < 3k tokens each |
