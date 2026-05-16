# SpecForge

Spec-driven development framework for Claude Code. Full lifecycle from product discovery to code review. Auto-sizes by complexity. Parallel execution. Learns from every PR.

```
DISCOVER → SPECIFY → DESIGN → PLAN → EXECUTE → PR → REVIEW
  (opt)             (opt)    (opt)
          ↘ discuss gray areas
```

## What it does

- **Auto-sizes** the pipeline by complexity — quick fixes skip the ceremony, complex features get the full treatment
- **Traceable requirements** — every requirement gets an ID (FEAT-001) that flows through design → tasks → commits → review
- **Parallel execution** — tasks run in isolated worktrees simultaneously, not sequentially
- **Learning loop** — REVIEW registers lessons that all future agents read before writing code
- **Pluggable** — works with Linear, GitHub Issues, or no tracker; Worktrunk, git worktree, or Task tool

## Installation

1. Clone or download this repo
2. Install as a Claude Code plugin via the plugin manager

Or manually copy `skills/specforge/` to your `~/.claude/skills/` directory.

## Project Setup

In your project root, run:
```
/specforge init project
```

This creates `.specforge/` with `project.yaml`, `STATE.md`, `PROJECT.md`, and `ROADMAP.md`.

For an existing codebase, run brownfield mapping first:
```
/specforge map codebase
```

## Usage

```
/specforge discover features          # Explore and propose features
/specforge specify ZEM-42             # Spec a feature from an issue
/specforge implement ZEM-42           # Full pipeline from spec to PR
/specforge quick fix {description}    # Express lane for small changes
/specforge review PR #123             # 4-perspective PR analysis
/specforge pause work                 # Save context for next session
/specforge resume work                # Pick up where you left off
```

## Pipeline

| Phase | Reference | Auto-triggered |
|---|---|---|
| Discover | `references/discover.md` | Manual |
| Specify | `references/specify.md` | Always |
| Discuss | `references/discuss.md` | When gray areas detected |
| Design | `references/design.md` | Complex features |
| Plan | `references/plan.md` | Medium/Complex |
| Execute | `references/execute.md` | Always |
| PR | `references/pr.md` | After Execute |
| Review | `references/review.md` | After PR |

## Project Configuration

`.specforge/project.yaml` controls:
- Where specs/plans are saved (SecondBrain path or in-repo `.specs/`)
- Issue tracker (Linear, GitHub Issues, or none)
- Execution backend (Worktrunk, git worktree, or Task tool)

See `templates/project.yaml` for the full reference.

## Directory Structure

```
.specforge/
├── project.yaml        # Stack, paths, adapters
├── STATE.md            # Decisions, blockers, lessons, deferred ideas
├── PROJECT.md          # Vision and goals
├── ROADMAP.md          # Features and milestones
└── architecture/       # Project-specific guides (DDD, conventions, etc.)
```

Specs, plans, and tasks are stored at the path configured in `project.yaml`.

## Differentiators

- **DISCOVER phase** — product exploration before spec; propose features with value/complexity analysis
- **PR with evidence** — automated PR with API test output and acceptance criteria checklist
- **REVIEW learning loop** — findings registered in STATE.md, read by future agents
- **Worktrunk-first** — true git worktree isolation for parallel task execution on Windows
- **Configurable artifact storage** — SecondBrain (Obsidian) or in-repo, your choice

## License

MIT
