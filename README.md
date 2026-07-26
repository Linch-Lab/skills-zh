# Skills-zh

[Matt Pocock 的 Skills](https://github.com/mattpocock/skills) 中文學習複刻。

## 這是什麼？

Matt Pocock 的 skills 是寫給 AI agent 用的「技能卡」——每張卡告訴 AI 如何完成一個特定任務。這套方法極其有效，但原文全是英文，中文 AI agent 很難直接採用。

這個 repo 是我**逐篇學習、用中文重寫** Matt Pocock 每一張 skill 的過程記錄。我會一邊理解原文的結構與設計哲學，一邊用中文重新表達，並在 **Hermes Agent + DeepSeek API** 的環境中實測驗證。

## 為什麼要這樣做？

| 原始 (mattpocock/skills) | 這裡 (skills-zh) |
|---|---|
| 英文，面向 Cursor / Copilot | 繁體中文，面向 Hermes Agent |
| Claude / GPT 語境 | DeepSeek 語境 |
| 主題偏前端開發 | 涵蓋更廣（開發、自動化、筆記、個人管理） |
| 學習門檻：英文 + Claude 生態 | 學習門檻：只需要中文 |

## 目錄結構

```
skills-zh/
├── README.md
├── skills/
│   ├── 01-xxx/          # 每篇 skill 一個目錄
│   │   ├── SKILL.md     # 中文重寫的 skill
│   │   └── notes.md     # 學習筆記：原文 vs 中文版本的差異
│   └── ...
└── tests/               # 驗證腳本
```

## 測試環境

- **Agent 框架**：[Hermes Agent](https://github.com/NousResearch/hermes-agent)（Nous Research）
- **LLM**：DeepSeek API（deepseek-v4-pro / deepseek-v4-flash）
- **作業系統**：Linux (Ubuntu 24.04)
- **Shell**：Bash

每篇 skill 重寫後，會在 Hermes Agent 中載入並實際執行，確認 AI 能正確理解並完成任務。

## 授權

MIT — 同 Matt Pocock 原始 repo。
