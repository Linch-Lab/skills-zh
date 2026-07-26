---
name: triage
description: 此 skill 禁止自動調用。將 issue 和外部 PR 移過分類角色的狀態機——分類、驗證、必要時詰問、撰寫 agent-ready 簡報。
---

# 分類

將專案議題追蹤系統上的 issue 移過一個小型分類角色狀態機。

若此 repo 將外部 PR 視為請求介面，分類也涵蓋它們：PR 是附帶程式碼的 issue——相同角色、相同狀態、相同狀態機。

分類期間發布到議題追蹤系統的每個留言或 issue 必須以此免責聲明開頭：`> *此為 AI 在分類期間產生。*`

## 角色

兩個類別角色：`bug`、`enhancement`。

五個狀態角色：`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`。

## 流程

1. 收集上下文。閱讀完整 issue/PR，解析先前的分類筆記，探索程式碼庫。
2. 建議。告訴維護者你的類別和狀態建議及理由。
3. 驗證主張。重現 bug，確認 diff。
4. 詰問（若需要）。執行 `/grilling` 和 `/domain-modeling`。
5. 套用結果。撰寫 agent 簡報或其他對應行動。

詳見 AGENT-BRIEF.md 和 OUT-OF-SCOPE.md。
