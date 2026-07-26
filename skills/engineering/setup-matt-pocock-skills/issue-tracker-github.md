# 議題追蹤系統：GitHub

本 repo 的 issue 和 PRD 以 GitHub issue 形式存放。所有操作使用 `gh` CLI。

## 慣例

- **建立 issue**：`gh issue create --title "..." --body "..."`。多行 body 使用 heredoc。
- **讀取 issue**：`gh issue view <number> --comments`，用 `jq` 過濾留言並一併取得標籤。
- **列出 issue**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`，搭配適當的 `--label` 和 `--state` 過濾。
- **對 issue 留言**：`gh issue comment <number> --body "..."`
- **套用 / 移除標籤**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **關閉**：`gh issue close <number> --comment "..."`

從 `git remote -v` 推斷 repo——在 clone 目錄內執行時 `gh` 會自動處理。

## Pull requests 作為分類介面

**PRs as a request surface：否。**（若本 repo 將外部 PR 視為功能請求則設為 `是`；`/triage` 會讀取此標誌。）

設為 `是` 時，PR 會經過與 issue 相同的標籤和狀態，使用 `gh pr` 對應指令：

- **讀取 PR**：`gh pr view <number> --comments` 以及 `gh pr diff <number>` 取得 diff。
- **列出待分類的外部 PR**：`gh pr list --state open --json number,title,body,labels,author,authorAssociation,comments`，只保留 `authorAssociation` 為 `CONTRIBUTOR`、`FIRST_TIME_CONTRIBUTOR` 或 `NONE` 的項目（排除 `OWNER`/`MEMBER`/`COLLABORATOR`）。
- **留言 / 標籤 / 關閉**：`gh pr comment`、`gh pr edit --add-label`/`--remove-label`、`gh pr close`。

GitHub 的 issue 和 PR 共用同一編號空間，因此 `#42` 可能是 issue 也可能是 PR——用 `gh pr view 42` 確認，失敗時改用 `gh issue view 42`。

## 當 skill 說「發布到議題追蹤系統」

建立一個 GitHub issue。

## 當 skill 說「取得相關票據」

執行 `gh issue view <number> --comments`。

## Wayfinding 操作

由 `/wayfinder` 使用。**地圖**是一張 issue，**子票據**為其下層 issue。

- **地圖**：一張標記為 `wayfinder:map` 的 issue，內含 Notes / Decisions-so-far / Fog 正文。`gh issue create --label wayfinder:map`。
- **子票據**：一張連結到地圖的 issue，透過 GitHub sub-issue 機制（`gh api` 操作 sub-issues 端點）。若 sub-issues 未啟用，將子票據加入地圖正文的任務清單，並在子票據正文開頭加上 `Part of #<map>`。標籤：`wayfinder:<type>`（`research`/`prototype`/`grilling`/`task`）。被領取後，指派給執行開發者。
- **阻塞**：GitHub 的**原生 issue 依賴**——正規、UI 可見的表示方式。使用 `gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>` 加入依賴邊，其中 `<blocker-db-id>` 是阻塞者的數字**資料庫 ID**（`gh api repos/<owner>/<repo>/issues/<n> --jq .id`，不是 `#number` 或 `node_id`）。GitHub 回報 `issue_dependencies_summary.blocked_by`（僅開啟中的阻塞者——即有效閘門）。若依賴功能不可用，改用子票據正文開頭的 `Blocked by: #<n>, #<n>` 行。當所有阻塞者皆關閉時，票據即解除阻塞。
- **前線查詢**：列出地圖的開啟中子票據（`gh issue list --state open`，限於地圖的 sub-issues / 任務清單），排除有開啟中阻塞者（`issue_dependencies_summary.blocked_by > 0`，或 `Blocked by` 行中有開啟中的 issue）或已指派者的項目；以地圖順序的第一個為準。
- **領取**：`gh issue edit <n> --add-assignee @me`——該 session 的第一次寫入。
- **解決**：`gh issue comment <n> --body "<answer>"`，然後 `gh issue close <n>`，最後將上下文指針（gist + 連結）附加到地圖的 Decisions-so-far。
