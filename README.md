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

Skills: `/write`, `/fix`, `/review`, `/refactor`, `/debug`, `/spec`

## Skills

### Workflow Skills (manually invoked)

| Skill | Usage |
|-------|-------|
| `/write <feature>` | Implement a feature with TDD |
| `/fix <bug>` | Fix a bug (diagnose → Red → Green → Refactor) |
| `/review [--staged \| path]` | Review local changes (style, tests, architecture) |
| `/refactor [path \| module]` | Safe refactoring with smell analysis and TDD verification |
| `/debug <error>` | Systematic root cause investigation before any fix |
| `/spec <feature>` | Define interface contract (Given/When/Then + TypeScript interface + invariants) |

### Methodology Skills (auto-loaded by workflows)

These are not shown in the `/` menu. Claude loads them automatically when needed.

| Skill | When auto-loaded |
|-------|-----------------|
| `principles` | Designing features, architecture decisions, SOLID violations |
| `testing` | Implementing features, fixing bugs, changing behavior |
| `done` | End of any workflow that produces code changes |

### Which skills each command loads

| Command | `spec` | `principles` | `testing` | `debug` | `done` |
|---------|:---:|:---:|:---:|:---:|:---:|
| `/write` | if Spec Gate¹ | always | always | — | always |
| `/fix` | — | if design problem | always | always | always |
| `/review` | — | always | always | — | — |
| `/refactor` | — | if SOLID violation | always | — | always |

> ¹ **Spec Gate** — three questions before writing code: (1) Is this a bug fix or internal change? (2) Does a TypeScript interface already exist? (3) Can you name 3+ boundary cases immediately? If all YES → skip spec, go straight to TDD. Any NO → load `spec` first.

## How It Works

**Two layers** — workflow skills provide step-by-step processes, methodology skills provide knowledge.

- **Workflow skills** use numbered steps with explicit confirmation gates — Claude won't write code until you approve the plan
- **Methodology skills** are auto-loaded by workflows based on context (e.g., `/fix` always loads `testing`, optionally loads `principles` if the root cause is structural)
- `disable-model-invocation: true` on workflow skills — no accidental auto-triggering

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
