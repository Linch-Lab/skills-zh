# 撰寫 Agent 簡報

Agent 簡報是 issue 或 PR 移至 `ready-for-agent` 時發布的結構化留言。它是 AFK agent 將依據的權威規格。

## 原則

### 耐久性優先於精確性

issue 可能在 `ready-for-agent` 中放置數天或數週。撰寫簡報使其在檔案被重新命名、移動或重構後仍有用。

- 描述介面、型別和行為合約；不要引用檔案路徑或行號。

### 行為性，非程序性

描述系統應該做什麼，而非如何實作。

### 完整的接受條件

每個 agent 簡報必須有具體、可測試的接受條件。

### 明確的範圍邊界

說明超出範圍的內容。

## 模板

```markdown
## Agent Brief

**Category:** bug / enhancement
**Summary:** 一行描述

**Current behavior:** 現在發生什麼

**Desired behavior:** 完成後應該發生什麼

**Key interfaces:** 相關型別和介面

**Acceptance criteria:** 可測試的條件清單

**Out of scope:** 不應改變的東西
```
