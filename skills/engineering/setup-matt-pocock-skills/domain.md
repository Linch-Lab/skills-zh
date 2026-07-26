# 領域文件

工程類 skills 在探索程式碼庫時應如何讀取本 repo 的領域文件。

## 探索前，先閱讀這些

- repo 根目錄的 **`CONTEXT.md`**，或
- repo 根目錄的 **`CONTEXT-MAP.md`**（若存在）——它指向每個情境各自的 `CONTEXT.md`。閱讀與主題相關的各個檔案。
- **`docs/adr/`**——閱讀與你即將處理的區域相關的 ADR。在多重情境 repo 中，也檢查 `src/<context>/docs/adr/` 中的情境範圍決策。

若上述任一檔案不存在，**默默繼續**。不要標記其缺失；不要主動建議建立它們。`/domain-modeling` skill（透過 `/grill-with-docs` 和 `/improve-codebase-architecture` 觸發）會在術語或決策實際被解決時延遲建立它們。

## 檔案結構

單一情境 repo（大多數 repo）：

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

多重情境 repo（根目錄存在 `CONTEXT-MAP.md`）：

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← 系統層級決策
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← 情境特定決策
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

## 使用詞彙表的用語

當你的輸出命名一個領域概念時（issue 標題、重構提案、假設、測試名稱），使用 `CONTEXT.md` 中定義的術語。不要偏離到詞彙表明確避免的同義詞。

若你需要的概念尚不在詞彙表中，這是信號——要嘛你在發明專案不使用的語言（重新考慮），要嘛存在真正的缺口（記錄下來供 `/domain-modeling` 使用）。

## 標記 ADR 衝突

若你的輸出與現有 ADR 矛盾，明確指出而非默默覆蓋：

> _與 ADR-0007（event-sourced orders）矛盾——但值得重新檢視，因為…_
