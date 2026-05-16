# Coding Principles

Behavioral rules for implementation. Applied during EXECUTE by every subagent.

## Knowledge Verification Chain

Before any technical decision, follow this chain in strict order. Never skip.

```
Step 1: Existing codebase
        → check .specforge/architecture/ for established patterns
        → grep for existing implementations of what you need
        → reuse before creating

Step 2: Project docs
        → README, inline comments, migration files
        → .specforge/STATE.md decisions and lessons

Step 3: Official docs / web
        → library documentation for unfamiliar APIs
        → language spec for subtle behavior

Step 4: Flag as uncertain
        → "I'm not certain about X — here's my reasoning, but verify before shipping"
```

**Never fabricate.** Inventing an API, pattern, or behavior that doesn't exist cascades failures through the entire pipeline — wrong design → wrong tasks → broken implementation. If you can't find it in steps 1-3, say you don't know. Uncertainty is always cheaper than a wrong answer.

## Surgical Changes

Touch only what the task requires. Nothing else.

- Do NOT improve adjacent code that wasn't broken
- Do NOT refactor things that aren't in your task
- Do NOT add "while I'm here" changes
- Do NOT improve comments, formatting, or naming beyond your task scope

When your changes create orphans (unused imports, dead variables): remove them.
When you notice pre-existing dead code: mention it in your completion report — do not delete it.

## Simplicity

Write the minimum code that satisfies the task's Done When criterion.

- No speculative features ("this might be useful later")
- No premature abstractions (three similar lines is better than a wrong abstraction)
- No extra configuration points that weren't asked for
- No backwards-compatibility shims for code you just wrote

Test: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## Test Integrity

Tests exist to catch regressions. Never weaken them.

- Do NOT delete tests that fail
- Do NOT loosen assertions to make tests pass (changing `assert.Equal` to `assert.NotNil`)
- Do NOT mock away the thing being tested
- Do NOT write tests that always pass regardless of implementation

If a test fails and you believe the test is wrong: report it, do not change it unilaterally.

## Atomic Commits

One task = one commit. Never more.

Format: `{type}({scope}): {description}`

Types: `feat`, `fix`, `refactor`, `test`, `chore`, `docs`

Examples:
```
feat(payments): add CreatePayment use case with amount validation
fix(api): handle missing field in JSON response mapping
refactor(auth): extract token claims to dedicated type
```

Do NOT use `git add -A` blindly. Stage only the files your task touched.

## Error Handling

- Every error must be handled — no silent swallows
- Return errors upward; don't log-and-continue in business logic
- Domain errors are typed named values, not generic strings — follow the pattern in `.specforge/architecture/conventions`
- Presentation layer (HTTP handlers, controllers) translates domain errors to response codes — business logic does not know about HTTP
- Never return internal error details (stack traces, query errors) to external clients

## Security Defaults

- Validate all inputs at the API boundary — never trust client-supplied IDs
- Users can only access their own data — always filter by user ID
- Never log passwords, tokens, or PII
- Use parameterized queries — never interpolate user input into SQL strings
