# 好測試與壞測試

## 好測試

**整合風格**：透過真實介面測試，而非模擬內部組件。

```typescript
// 好：測試可觀察的行為
test("user can checkout with valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

特徵：

- 測試使用者/呼叫者關心的行為
- 只使用公開 API
- 內部重構後仍存活
- 描述做什麼（WHAT），而非怎麼做（HOW）
- 每個測試一個邏輯斷言

## 壞測試

**實作細節測試**：耦合到內部結構。

```typescript
// 壞：測試實作細節
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

危險信號：

- 模擬內部協作者
- 測試私有方法
- 斷言呼叫次數/順序
- 重構時行為沒變但測試掛了
- 測試名稱描述 HOW 而非 WHAT
- 透過外部手段而非介面驗證

```typescript
// 壞：繞過介面驗證
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// 好：透過介面驗證
test("createUser makes user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

**套套邏輯測試**：預期值重新陳述實作邏輯，因此測試在構造上就成立。

```typescript
// 壞：預期值以跟程式碼相同的方式計算
test("calculateTotal sums line items", () => {
  const items = [{ price: 10 }, { price: 5 }];
  const expected = items.reduce((sum, i) => sum + i.price, 0);
  expect(calculateTotal(items)).toBe(expected);
});

// 好：預期值是獨立的已知字面值
test("calculateTotal sums line items", () => {
  expect(calculateTotal([{ price: 10 }, { price: 5 }])).toBe(15);
});
```
