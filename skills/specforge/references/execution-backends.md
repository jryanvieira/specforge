# Execution Backends

Commands for each execution backend. Read `project.yaml` → `execution.backend` to select the right section.

## Worktrunk (Windows — default)

Worktrunk (`git-wt`) manages worktrees and spawns Claude agents. Binary at `%LOCALAPPDATA%\Programs\worktrunk\git-wt.exe`.

**Create worktree and spawn subagent:**
```powershell
git-wt switch --create {branch-name} -x claude -- "{instruction}"
```

**List active worktrees:**
```powershell
git-wt list
```

**Merge after PR is approved (run from repo root):**
```powershell
git-wt merge main --branch {branch-name}
```

**Remove worktree after merge:**
```powershell
git-wt remove {branch-name}
```

**Branch naming convention:**
```
{tracker-prefix}/{ISSUE-ID}-{short-description-kebab}
```
Examples: `task/ZEM-42-create-goal`, `feat/GH-12-export-pdf`

**Notes:**
- Always run from the repository root (not inside a worktree)
- The `--create` flag creates the branch from `main` automatically
- The `-x claude` flag spawns a new Claude Code instance in the worktree

---

## Git Worktree (Unix/Windows — raw)

Use when Worktrunk is not available. Subagent is spawned manually.

**Create worktree:**
```bash
git worktree add ../{repo-name}-{branch-short} -b {branch-name}
```

**List active worktrees:**
```bash
git worktree list
```

**Remove worktree after merge:**
```bash
git worktree remove ../{repo-name}-{branch-short}
git branch -d {branch-name}
```

**Spawn subagent** (run from inside the worktree directory):
```bash
cd ../{repo-name}-{branch-short}
claude "{instruction}"
```

**Notes:**
- Worktrees are sibling directories of the main repo
- Each worktree has its own working directory but shares `.git`
- Avoid running `git checkout` inside worktrees

---

## Task Tool (Generic — no isolation)

Use when neither Worktrunk nor git worktree is appropriate, or for lightweight parallel execution without branch isolation.

**Create branch and delegate:**
```
Use the Task tool (or equivalent sub-agent mechanism) with:
- A new branch created via: git checkout -b {branch-name}
- The full instruction block from execute.md
- The subagent reports back with status, files changed, and gate result
```

**Notes:**
- No physical isolation — subagents share the same filesystem
- Parallel tasks must touch different files (enforced by the plan)
- Best for Cloud/API-based execution environments

---

## Selecting the Backend

The orchestrator reads `project.yaml` at the start of EXECUTE:

```yaml
execution:
  backend: worktrunk   # → use Worktrunk section above
  platform: windows    # → use PowerShell syntax
```

| backend value | Platform | Use section |
|---|---|---|
| `worktrunk` | windows | Worktrunk |
| `git-worktree` | unix | Git Worktree |
| `git-worktree` | windows | Git Worktree (PowerShell paths) |
| `task-tool` | any | Task Tool |
