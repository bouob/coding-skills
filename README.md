# coding-skills

A [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code/plugins) for opinionated TypeScript, React, and Python development — TDD workflows, SOLID principles, and structured code review.

## Install

```bash
# as a plugin (recommended)
claude plugin add ./coding-skills

# or during development
claude --plugin-dir ./coding-skills
```

Commands become `/coding:write`, `/coding:fix`, etc.

## Skills

Auto-loaded by Claude based on context. No manual invocation needed.

| Skill | Triggers when... |
|-------|-----------------|
| `principles` | Designing features, architecture decisions, interface boundaries |
| `testing` | Implementing features, fixing bugs, changing behavior |

## Commands

| Command | Usage |
|---------|-------|
| `/coding:write <feature>` | Implement a feature with TDD |
| `/coding:fix <bug>` | Fix a bug (diagnose → Red → Green → Refactor) |
| `/coding:review [--staged \| path]` | Review local changes (style, tests, architecture) |
| `/coding:refactor [path \| module]` | Safe refactoring with smell analysis and TDD verification |

## How It Works

**Two layers** — skills provide knowledge, commands provide workflows.

- **Skills** have an **Applicability Rubric** (when to use) and a **Completion Rubric** (how to verify)
- **Commands** use numbered steps with explicit confirmation gates — Claude won't write code until you approve the plan
- Commands dynamically load skills based on context (e.g., `/coding:fix` always loads `testing`, optionally loads `principles` if the root cause is structural)
- `disable-model-invocation: true` on all commands — no accidental auto-triggering

## Workflow

```
/coding:write "add user auth"  →  plan  →  confirm  →  TDD cycles  →  /coding:review
/coding:fix "login crash"      →  diagnose  →  confirm  →  Red/Green/Refactor  →  /coding:review
/coding:refactor src/auth/     →  smell analysis  →  confirm  →  incremental transforms  →  /coding:review
```

## License

MIT
