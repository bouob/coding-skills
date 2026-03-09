---
name: write
description: >
  Implement a small to medium feature with TDD approach.
  For large features requiring architecture exploration, use /feature-dev instead.
argument-hint: [feature description]
disable-model-invocation: true
allowed-tools: Read, Edit, Write, Grep, Glob,
  Bash(git log:*), Bash(git diff:*), Bash(git show:*),
  Bash(npm test:*), Bash(npm run test:*), Bash(npx vitest:*),
  Skill
---

# /write — Feature Implementation Workflow

## Skills Rubric

| Skill | When to load |
|-------|-------------|
| **principles** | Always — design decisions are part of every feature |
| **testing** | Always when the feature introduces new behavior (almost always) |

## Step 1: Understand the Context

Feature request: $ARGUMENTS

Read the relevant files before writing any code.

- Identify which files / modules will be affected
- Look at existing patterns in adjacent code (`git log --oneline -10` for recent context)
- Do NOT read framework source code — only application code

## Step 2: Load Skills

Apply the Skills Rubric above. Load both skills for any feature with new behavior.

## Step 3: Plan Mode — TDD Task Breakdown

Enter plan mode. Break the feature into the smallest meaningful tasks.

- Apply minimum change principle: prefer extending over modifying
- Each task should be completable as one Red/Green/Refactor cycle
- List any questions or assumptions for the user to confirm

**Wait for user confirmation before writing any code.**

## Step 4: Implement (Red → Green → Refactor per task)

For each task in the breakdown:

**Red** — Write a failing test that describes the behavior.
- Confirm: "Test fails because [reason]."

**Green** — Write minimum code to pass the test.
- Confirm: "All tests pass."

**Refactor** — Clean up without changing behavior.
- Confirm: "Tests still pass."

## Step 5: Completion Check

Verify against both skill rubrics:

- **principles rubric** — naming, types, function length, import order
- **testing rubric** — AAA structure, single assertion, deterministic, integration path exists

---

Next: run `/review` to verify style consistency and test quality before committing.
