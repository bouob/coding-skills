# coding-skills

A [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code/plugins) for opinionated TypeScript, React, and Python development — TDD workflows, SOLID principles, and structured code review.

## Install

```bash
# add as marketplace + install (recommended)
/plugin marketplace add bouob/coding-skills
/plugin install coding-skills

# or load directly during development
claude --plugin-dir ./coding-skills
```

Commands: `/write`, `/fix`, `/review`, `/refactor`

## Skills

Auto-loaded by Claude based on context. No manual invocation needed.

| Skill | Triggers when... |
|-------|-----------------|
| `principles` | Designing features, architecture decisions, interface boundaries |
| `testing` | Implementing features, fixing bugs, changing behavior |

## Commands

| Command | Usage |
|---------|-------|
| `/write <feature>` | Implement a feature with TDD |
| `/fix <bug>` | Fix a bug (diagnose → Red → Green → Refactor) |
| `/review [--staged \| path]` | Review local changes (style, tests, architecture) |
| `/refactor [path \| module]` | Safe refactoring with smell analysis and TDD verification |

## How It Works

**Two layers** — skills provide knowledge, commands provide workflows.

- **Skills** have an **Applicability Rubric** (when to use) and a **Completion Rubric** (how to verify)
- **Commands** use numbered steps with explicit confirmation gates — Claude won't write code until you approve the plan
- Commands dynamically load skills based on context (e.g., `/fix` always loads `testing`, optionally loads `principles` if the root cause is structural)
- `disable-model-invocation: true` on all commands — no accidental auto-triggering

## Workflow

```
/write "add user auth"  →  plan  →  confirm  →  TDD cycles  →  /review
/fix "login crash"      →  diagnose  →  confirm  →  Red/Green/Refactor  →  /review
/refactor src/auth/     →  smell analysis  →  confirm  →  incremental transforms  →  /review
```

## License

MIT
