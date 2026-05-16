# Issue Tracker Adapters

Commands for each issue tracker type. Read `project.yaml` → `issue_tracker.type` to select the right section.

## Linear (default)

Uses MCP Linear tools. Workspace defined in `project.yaml` → `issue_tracker.workspace`.

**Fetch issue:**
```
mcp linear get_issue { id: "{ISSUE-ID}" }
```

**Update issue description (add spec/plan link):**
```
mcp linear save_issue { id: "{ISSUE-ID}", description: "{existing description}\n\n**Spec:** {path}" }
```

**Update issue status:**
```
mcp linear save_issue { id: "{ISSUE-ID}", status: "In Progress" }
mcp linear save_issue { id: "{ISSUE-ID}", status: "In Review" }
mcp linear save_issue { id: "{ISSUE-ID}", status: "Done" }
```

**Create sub-issue (task):**
```
mcp linear save_issue {
  title: "TASK-N: {task name}",
  description: "{full task content}",
  parent_id: "{parent ISSUE-ID}"
}
```

**Add comment:**
```
mcp linear save_comment { issue_id: "{ISSUE-ID}", body: "{comment}" }
```

**Status name mapping (check your workspace):**
Common values: "Backlog", "Todo", "In Progress", "In Review", "Done", "Cancelled"

---

## GitHub Issues

Uses `gh` CLI.

**Fetch issue:**
```bash
gh issue view {NUMBER} --json title,body,labels,assignees
```

**Update issue (add comment with spec link):**
```bash
gh issue comment {NUMBER} --body "Spec: {path}"
```

**Apply label as status:**
```bash
gh issue edit {NUMBER} --add-label "in-progress"
gh issue edit {NUMBER} --add-label "in-review" --remove-label "in-progress"
gh issue close {NUMBER}
```

**Create sub-task (as linked issue):**
```bash
gh issue create --title "TASK-N: {task name}" --body "{task content}\n\nParent: #{parent-number}"
```

---

## None (manual)

No issue tracker configured. Skip all tracker update steps. The orchestrator still tracks status internally during the session.

---

## Selecting the Adapter

The orchestrator reads `project.yaml` at startup:

```yaml
issue_tracker:
  type: linear          # → use Linear section
  workspace: myworkspace
```

| type value | Use section |
|---|---|
| `linear` | Linear |
| `github-issues` | GitHub Issues |
| `none` | None |
