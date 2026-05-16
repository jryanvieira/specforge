# PR — Pull Request Creation

Open a well-documented PR with evidence for each task branch.

## Pre-Work

1. Confirm you are on the task branch (not main)
2. Confirm Gate passes: run the gate command from the task definition
3. Confirm branch is up to date with main: `git fetch origin && git log HEAD..origin/main --oneline`
4. Confirm commits are clean: `git log --oneline origin/main..HEAD`

## Evidence Collection (for API features)

Before opening the PR, collect evidence that the feature works. This is mandatory for any backend endpoint or business logic change.

**Quick API test script** (adapt URL, method, and payload per feature):

```javascript
// Run in browser console or Node.js
const BASE = "http://localhost:{PORT}";
const TOKEN = "{JWT_TOKEN}"; // obtain from login endpoint first

async function test(label, method, path, body) {
  const res = await fetch(BASE + path, {
    method,
    headers: { "Content-Type": "application/json", "Authorization": "Bearer " + TOKEN },
    body: body ? JSON.stringify(body) : undefined
  });
  const data = await res.json().catch(() => res.text());
  console.log(`[${res.status}] ${label}:`, data);
  return { status: res.status, data };
}

// Add test calls here
await test("happy path", "POST", "/api/v1/{resource}", { /* payload */ });
await test("invalid input", "POST", "/api/v1/{resource}", { /* bad payload */ });
await test("not found", "GET", "/api/v1/{resource}/nonexistent-id", null);
```

Record the output. Include it in the PR description.

## PR Description Template

```markdown
## What was done

{2-3 sentences describing the change. Focus on behavior, not implementation.}

## Issue

[{ISSUE-ID}]({issue-tracker-url})

## Changes by bounded context

**{ContextName}:**
- `{file-path}` — {what changed}
- `{file-path}` — {what changed}

## How to test

1. {Step to reproduce the scenario}
2. {Expected result}

## Acceptance criteria

- [ ] {PREFIX}-001: {criterion from spec}
- [ ] {PREFIX}-002: {criterion from spec}

## Evidence

```
{paste test output here}
```

## Technical checklist

- [ ] All tests pass (`{gate command}`)
- [ ] Money values in integer cents (never float)
- [ ] No domain layer importing infrastructure
- [ ] UTC timestamps throughout
- [ ] Error types defined in context's errors.go
- [ ] No unused imports
```

## Opening the PR

**PowerShell (Windows):**
```powershell
gh pr create `
  --title "{type}({scope}): {concise title}" `
  --body "$(cat <<'EOF'
{PR description content}
EOF
)" `
  --base main `
  --head {branch-name}
```

**Bash (Unix):**
```bash
gh pr create \
  --title "{type}({scope}): {concise title}" \
  --body "$(cat <<'EOF'
{PR description content}
EOF
)" \
  --base main \
  --head {branch-name}
```

## Post-PR Actions

1. Note the PR URL from `gh pr create` output
2. Update issue tracker:
   - Add PR link to issue description or as a comment
   - Change status to "In Review"
   - See [issue-tracker-adapters.md](issue-tracker-adapters.md)
3. Report to user: "PR #{number} opened: {url}"

## Multiple PRs (parallel tasks)

When multiple task branches exist, open a PR for each independently. List all URLs in the final report:

```
PRs opened:
- #{N} — {title} → {url}
- #{N} — {title} → {url}
```

**NEVER merge a PR.** That is the human's responsibility after review.
