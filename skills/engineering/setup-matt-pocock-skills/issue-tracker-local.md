# 議題追蹤系統：本地 Markdown

本 repo 的 issue 和規格書（即 PRD）以 markdown 檔案形式存放在 `.scratch/` 中。

## 慣例

- 每個功能一個目錄：`.scratch/<feature-slug>/`
- 規格書為 `.scratch/<feature-slug>/spec.md`
- 實作 issue 每張票據一個檔案，位於 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，從 `01` 開始編號——絕不使用單一合併票據檔案
- 分類狀態以 `Status:` 行記錄在各 issue 檔案開頭附近（角色字串見 `triage-labels.md`）
- 留言和對話歷史附加在檔案底部 `## Comments` 標題下

## 當 skill 說「發布到議題追蹤系統」

在 `.scratch/<feature-slug>/` 下建立新檔案（必要時建立目錄）。

## 當 skill 說「取得相關票據」

讀取參考路徑的檔案。使用者通常會直接傳入路徑或 issue 編號。

## Wayfinding 操作

由 `/wayfinder` 使用。**地圖**是一個檔案，每個**子票據**各一個檔案。

- **地圖**：`.scratch/<effort>/map.md`——Notes / Decisions-so-far / Fog 正文。
- **子票據**：`.scratch/<effort>/issues/NN-<slug>.md`，從 `01` 開始編號，問題放在正文中。`Type:` 行記錄票據類型（`research`/`prototype`/`grilling`/`task`）；`Status:` 行記錄 `claimed`/`resolved`。
- **阻塞**：檔案開頭附近的 `Blocked by: NN, NN` 行。當所列的每個檔案皆為 `resolved` 時，票據即解除阻塞。
- **前線**：掃描 `.scratch/<effort>/issues/` 中開啟、未阻塞、未領取的檔案；以編號順序第一個為準。
- **領取**：設定 `Status: claimed` 並儲存，再開始任何工作。
- **解決**：在 `## Answer` 標題下附加答案，設定 `Status: resolved`，然後將上下文指針（gist + 連結）附加到 `map.md` 中地圖的 Decisions-so-far。
