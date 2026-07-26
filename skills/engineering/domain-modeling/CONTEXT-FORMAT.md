# CONTEXT.md 格式

## 結構

```md
# {情境名稱}

{一兩句話描述此情境是什麼以及為何存在。}

## Language

**Order**：
{一兩句話描述此術語}
_Avoid_：Purchase, transaction

**Invoice**：
送達後發送給客戶的付款請求。
_Avoid_：Bill, payment request

**Customer**：
下訂單的個人或組織。
_Avoid_：Client, buyer, account
```

## 規則

- **要有立場。** 當同一概念存在多個詞時，挑選最佳的一個，將其餘列在 `_Avoid_` 下。
- **定義保持精簡。** 最多一兩句話。定義它是什麼，而非它做什麼。
- **只包含此專案情境界定的術語。** 通用程式設計概念（逾時、錯誤型別、工具模式）即使專案大量使用也不屬於此。在加入一個術語前，問自己：這是此情境獨有的概念，還是通用程式設計概念？只有前者才屬於這裡。
- **當自然群集出現時，以子標題分組。** 若所有術語屬於單一內聚領域，扁平列表即可。

## 單一 vs 多重情境 repo

**單一情境（大多數 repo）：** repo 根目錄一個 `CONTEXT.md`。

**多重情境：** repo 根目錄的 `CONTEXT-MAP.md` 列出各情境、其位置及彼此關係：

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md) — 接收並追蹤客戶訂單
- [Billing](./src/billing/CONTEXT.md) — 產生發票並處理付款
- [Fulfillment](./src/fulfillment/CONTEXT.md) — 管理倉庫揀貨與出貨

## Relationships

- **Ordering → Fulfillment**：Ordering 發出 `OrderPlaced` 事件；Fulfillment 消費它以開始揀貨
- **Fulfillment → Billing**：Fulfillment 發出 `ShipmentDispatched` 事件；Billing 消費它以產生發票
- **Ordering ↔ Billing**：共用 `CustomerId` 和 `Money` 型別
```

Skill 推斷適用哪種結構：

- 若 `CONTEXT-MAP.md` 存在，讀取它以找到各情境
- 若僅有根目錄 `CONTEXT.md`，單一情境
- 若兩者皆無，在第一個術語確定時延遲建立根目錄 `CONTEXT.md`

當存在多重情境時，推斷當前主題與哪個情境相關。若不清楚，詢問。
