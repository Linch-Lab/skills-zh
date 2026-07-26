---
name: resolving-merge-conflicts
description: 當你需要解決進行中的 git merge/rebase 衝突時使用。
---

1. 查看 merge/rebase 的當前狀態。檢查 git 歷史和衝突檔案。

2. 為每個衝突找到主要來源。深入理解每個變更為何做出，原始意圖是什麼。閱讀 commit 訊息、檢查 PR、檢查原始 issue/票據。

3. 解決每個區塊。盡可能保留雙方的意圖。不相容時，選擇符合 merge 陳述目標的那一個並註明取捨。不要發明新行為。永遠解決；永遠不 `--abort`。

4. 發現專案的自動化檢查並執行——通常先型別檢查、再測試、再格式化。修復 merge 破壞的任何東西。

5. 完成 merge/rebase。暫存所有內容並提交。若 rebase，繼續 rebase 過程直到所有 commit 被 rebase。
