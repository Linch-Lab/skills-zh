# 何時模擬

只在**系統邊界**模擬：

- 外部 API（支付、郵件等）
- 資料庫（有時——偏好測試資料庫）
- 時間/隨機性
- 檔案系統（有時）

不要模擬：

- 你自己的類別/模組
- 內部協作者
- 任何你控制的東西

## 為可模擬性設計

在系統邊界處，設計易於模擬的介面：

**1. 使用依賴注入**

將外部依賴傳入，而非在內部建立：

```typescript
// 容易模擬
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// 難以模擬
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}
```

**2. 偏好 SDK 風格介面，而非通用 fetcher**

為每個外部操作建立特定函式，而非一個帶條件邏輯的通用函式：

```typescript
// 好：每個函式可獨立模擬
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch('/orders', { method: 'POST', body: data }),
};

// 壞：模擬時需要在 mock 內寫條件邏輯
const api = {
  fetch: (endpoint, options) => fetch(endpoint, options),
};
```

SDK 風格的好處：
- 每個 mock 回傳一個特定形狀
- 測試設定中沒有條件邏輯
- 更容易看出測試用到哪些端點
- 每個端點的型別安全
