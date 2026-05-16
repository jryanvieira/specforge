# Discover — Product Exploration & Feature Proposal

Explore the codebase and issue tracker to surface and prioritize feature opportunities.

## When to Use

- No specific feature is defined yet
- Starting a new development cycle
- User wants recommendations on what to build next
- Backlog grooming before sprint planning

## Pre-Work

1. Load `.specforge/PROJECT.md` — understand vision, goals, constraints
2. Load `.specforge/ROADMAP.md` — understand current milestones and planned features
3. Load `.specforge/STATE.md` — check deferred ideas and existing blockers
4. Load `.specforge/architecture/` — understand current bounded contexts
5. If `issue_tracker.type` is set, fetch open issues (see [issue-tracker-adapters.md](issue-tracker-adapters.md))

## Exploration Protocol

### Step 1: Current State Assessment

Explore the codebase to understand:
- Which features are complete and stable
- Which areas have known gaps or limitations
- Which bounded contexts have thin coverage
- What the current user journey looks like end-to-end

Read the issue tracker for:
- Open bugs (signal of friction)
- Feature requests (signal of demand)
- Tech debt items (signal of risk)

### Step 2: Opportunity Identification

Identify 3-5 feature opportunities. For each, assess:

**Value:** What user problem does this solve? How frequently does it occur?
**Complexity:** Which bounded contexts are affected? New patterns needed?
**Why Now:** Why is this the right time to build this?
**Risk:** What's the downside if this goes wrong?

### Step 3: Proposal

Present proposals in this format:

```markdown
## Feature Proposal: {Title}

**Value:** {User problem + frequency + impact}
**Why Now:** {Timing rationale}
**Bounded Contexts:** {list}
**Complexity:** Quick | Simple | Medium | Complex
**Dependencies:** {other features or issues this depends on, or "none"}
**Estimated scope:** {rough size — 1 day / 3 days / 1 week}

**What it looks like:**
{2-3 sentences describing the user experience or API behavior after this feature ships}
```

Present proposals ranked by value-to-complexity ratio. Explain the ranking briefly.

## Rules

- Only propose features aligned with PROJECT.md vision
- No pure refactors as standalone features (unless stability is a stated goal)
- Bugs get their own category — separate from features
- Do not create issues in the tracker without explicit user approval
- Capture ideas the user declines as deferred ideas in STATE.md — not lost, just not now

## After Approval

When the user approves a feature:
1. Create issue in the issue tracker (see [issue-tracker-adapters.md](issue-tracker-adapters.md))
2. Record the decision in STATE.md (Decisions section)
3. Proceed to [specify.md](specify.md)
