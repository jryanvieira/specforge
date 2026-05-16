# Quick Mode — Express Lane

For small, obvious changes. Zero ceremony. Same quality standards.

## Eligibility Criteria

All of the following must be true:

- [ ] ≤3 files change
- [ ] Scope fits in one sentence
- [ ] No new patterns or abstractions introduced
- [ ] No new external dependencies
- [ ] No interface changes that affect other bounded contexts
- [ ] Change is obviously correct — no architectural decision needed

If ANY criterion fails: exit quick mode, start from [specify.md](specify.md).

**Examples of valid quick tasks:**
- Fix a typo in an error message
- Add a missing field to an existing struct
- Correct an off-by-one in a calculation
- Update a config value
- Add a missing nil check
- Fix a broken test assertion

**Examples that look quick but aren't:**
- "Just add a column" (requires migration, repository change, use case change, handler change — Medium)
- "Just rename this field" (may break JSON contracts, SQLite column mapping — assess first)
- "Just add validation" (may require new domain error type, new test scenarios — assess first)

## Process

1. **Describe** — state in one sentence what changes and why
2. **List files** — explicitly name the ≤3 files that will change
3. **Implement** — surgical change, nothing else
4. **Test** — run the gate for the affected area
5. **Commit** — one atomic commit

## Gate

Run the narrowest test scope that covers the change:

```bash
# Go backend — run only the affected package
cd backend && go test ./internal/{domain}/{context}/...

# If touching multiple packages
cd backend && go test ./...

# Frontend — run only the affected component tests (if test suite exists)
```

## Commit Format

```
{type}({scope}): {description}

# Examples:
fix(goal): correct minimum amount validation to reject zero values
chore(config): update JWT expiry from 24h to 48h
fix(expense): handle nil category pointer in response mapping
```

## Quick Task Record (optional)

For audit trail, log to `.specforge/STATE.md` under Todos (completed):

```markdown
- [x] {YYYY-MM-DD} Quick: {one-sentence description} — {commit hash}
```

Only required if the project explicitly tracks quick changes. Otherwise skip.
