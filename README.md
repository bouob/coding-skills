# coding-skills

[繁體中文](./README.zh-TW.md)

A [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code/plugins) for opinionated TypeScript, React, and Python development — spec-driven interface design, TDD workflows, SOLID principles, and structured code review.

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

Auto-loaded by commands based on context. Can also be invoked standalone.

| Skill | When auto-loaded | Standalone |
|-------|-----------------|------------|
| `spec` | `/write` when interface is undefined (Spec Gate triggered) | `/spec` |
| `principles` | Designing features, architecture decisions, SOLID violations | — |
| `testing` | Implementing features, fixing bugs, changing behavior | — |
| `debug` | Diagnosing bugs in `/fix` | `/debug` |
| `done` | End of any workflow that produces code changes | `/done` |

### Which skills each command loads

| Command | `spec` | `principles` | `testing` | `debug` | `done` |
|---------|:---:|:---:|:---:|:---:|:---:|
| `/write` | if Spec Gate¹ | always | always | — | always |
| `/fix` | — | if design problem | always | always | always |
| `/review` | — | always | always | — | — |
| `/refactor` | — | if SOLID violation | always | — | always |

> ¹ **Spec Gate** — three questions before writing code: (1) Is this a bug fix or internal change? (2) Does a TypeScript interface already exist? (3) Can you name 3+ boundary cases immediately? If all YES → skip spec, go straight to TDD. Any NO → load `spec` first.

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
/write "add user auth"
  → Spec Gate (interface defined? boundary cases clear?)
      YES → plan → confirm → TDD cycles → /review
      NO  → /spec (interface + invariants) → TDD cycles → /review

/fix "login crash"      →  diagnose  →  confirm  →  Red/Green/Refactor  →  /review
/refactor src/auth/     →  smell analysis  →  confirm  →  incremental transforms  →  /review
```

## License

MIT
