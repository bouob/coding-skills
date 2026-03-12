# coding-skills

[English](./README.md)

一個 [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code/plugins)，提供具有主觀設定的 TypeScript、React 與 Python 開發流程 — 包含規格驅動介面設計（SDD）、TDD 工作流程、SOLID 原則與結構化程式碼審查。

## 安裝

```bash
# 從 marketplace 新增並安裝（推薦）
/plugin marketplace add bouob/coding-skills
/plugin install coding-skills

# 或在開發時直接載入
claude --plugin-dir ./coding-skills
```

指令：`/write`、`/fix`、`/review`、`/refactor`

## Skills

由指令根據情境自動載入，也可單獨呼叫。

| Skill | 自動載入時機 | 單獨呼叫 |
|-------|------------|---------|
| `spec` | `/write` 時介面尚未定義（Spec Gate 觸發） | `/spec` |
| `principles` | 設計新功能、架構決策、SOLID 違反 | — |
| `testing` | 實作功能、修復錯誤、改變行為 | — |
| `debug` | 在 `/fix` 中診斷錯誤根因 | `/debug` |
| `done` | 任何產出程式碼變更的工作流程結尾 | `/done` |

### 各指令載入的 Skills

| 指令 | `spec` | `principles` | `testing` | `debug` | `done` |
|------|:---:|:---:|:---:|:---:|:---:|
| `/write` | Spec Gate 觸發時¹ | 總是 | 總是 | — | 總是 |
| `/fix` | — | 設計問題時 | 總是 | 總是 | 總是 |
| `/review` | — | 總是 | 總是 | — | — |
| `/refactor` | — | SOLID 違反時 | 總是 | — | 總是 |

> ¹ **Spec Gate** — 寫程式碼前的三個問題：(1) 這是 bug fix 或介面不變的修改嗎？(2) TypeScript interface 已存在嗎？(3) 現在就能列出 3 個以上的邊界案例嗎？三問全 YES → 跳過 spec 直接 TDD。任一 NO → 先載入 `spec`。

## 指令

| 指令 | 用途 |
|------|------|
| `/write <功能描述>` | 以 TDD 方式實作功能 |
| `/fix <錯誤描述>` | 修復錯誤（診斷 → Red → Green → Refactor） |
| `/review [--staged \| path]` | 審查本地變更（風格、測試、架構） |
| `/refactor [path \| module]` | 安全重構，含壞味道 (Code Smell) 分析與 TDD 驗證 |

## 運作原理

**兩層架構** — Skills 提供知識，指令提供工作流程。

- **Skills** 具有 **適用規則**（何時使用）與 **完成規則**（如何驗證）
- **指令** 使用編號步驟加上明確確認關卡 — Claude 在你核准計畫前不會撰寫任何程式碼
- 指令根據情境動態載入 Skills（例如 `/fix` 總是載入 `testing`，若根因涉及設計問題則額外載入 `principles`）
- 所有指令皆設定 `disable-model-invocation: true` — 不會意外自動觸發

## 工作流程

```
/write "新增使用者驗證"
  → Spec Gate（介面已定義？邊界案例清楚？）
      全 YES → 計畫 → 確認 → TDD 循環 → /review
      任一 NO → /spec（介面 + 不變量）→ TDD 循環 → /review

/fix "登入頁面當機"      →  診斷  →  確認  →  Red/Green/Refactor  →  /review
/refactor src/auth/      →  壞味道 (Code Smell) 分析  →  確認  →  逐步重構  →  /review
```

## 授權

MIT
