# 議題追蹤系統：GitLab

本 repo 的 issue 和 PRD 以 GitLab issue 形式存放。所有操作使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。

## 慣例

- **建立 issue**：`glab issue create --title "..." --description "..."`。多行 description 使用 heredoc。傳入 `--description -` 可開啟編輯器。
- **讀取 issue**：`glab issue view <number> --comments`。使用 `-F json` 取得機器可讀輸出。
- **列出 issue**：`glab issue list -F json`，搭配適當的 `--label` 過濾。
- **對 issue 留言**：`glab issue note <number> --message "..."`。GitLab 將留言稱為 "notes"。
- **套用 / 移除標籤**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多個標籤可用逗號分隔或重複旗標。
- **關閉**：`glab issue close <number>`。`glab issue close` 不接受關閉留言，因此先以 `glab issue note <number> --message "..."` 發布說明，再關閉。
- **Merge requests**：GitLab 將 PR 稱為 "merge requests"。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——形狀與 `gh pr ...` 相同，以 `mr` 取代 `pr`、`note`/`--message` 取代 `comment`/`--body`。

從 `git remote -v` 推斷 repo——在 clone 目錄內執行時 `glab` 會自動處理。

## Merge requests 作為分類介面

**MRs as a request surface：否。**（若本 repo 將外部 merge request 視為功能請求則設為 `是`；`/triage` 會讀取此標誌。）

設為 `是` 時，MR 會經過與 issue 相同的標籤和狀態，使用 `glab mr` 對應指令：

- **讀取 MR**：`glab mr view <number> --comments` 以及 `glab mr diff <number>` 取得 diff。
- **列出待分類的外部 MR**：`glab mr list -F json`，只保留作者非專案成員/擁有者的 MR（貢獻者的 MR，非維護者進行中的工作）。
- **留言 / 標籤 / 關閉**：`glab mr note`、`glab mr update --label`/`--unlabel`、`glab mr close`。

與 GitHub 不同，GitLab 將 issue 和 MR 分開編號，因此 `#42` 在明確是哪個介面後即無歧義。

## 當 skill 說「發布到議題追蹤系統」

建立一個 GitLab issue。

## 當 skill 說「取得相關票據」

執行 `glab issue view <number> --comments`。

## Wayfinding 操作

由 `/wayfinder` 使用。**地圖**是一張 issue，**子票據**為其下層 issue。

- **地圖**：一張標記為 `wayfinder:map` 的 issue，內含 Notes / Decisions-so-far / Fog 正文。`glab issue create --label wayfinder:map`。（在具備原生 epic 的 GitLab 版本中，epic 可代為容納地圖；標記 issue 的方法在所有版本皆適用。）
- **子票據**：一張 issue，正文開頭為 `Part of #<map>`，標籤為 `wayfinder:<type>`（`research`/`prototype`/`grilling`/`task`）。被領取後，指派給執行開發者。
- **阻塞**：GitLab 的**原生 blocking link**——正規、UI 可見的表示方式。使用 `/blocked_by #<n>` 快速操作加入，以 note 形式發布（`glab issue note <child> --message "/blocked_by #<blocker>"`）。原生 blocking link 為 Premium/Ultimate 功能；在免費版（或不可用時）改用正文開頭的 `Blocked by: #<n>, #<n>` 行。當所有阻塞者皆關閉時，票據即解除阻塞。
- **前線查詢**：`glab issue list -F json` 限於地圖的子票據，排除有開啟中阻塞者的項目——原生 `blocked_by` 連結到開啟中 issue（`glab api projects/:id/issues/:iid/links`），或 `Blocked by` 行中有開啟中的 issue——或已指派者；以地圖順序的第一個為準。
- **領取**：`glab issue update <n> --assignee @me`——該 session 的第一次寫入。
- **解決**：`glab issue note <n> --message "<answer>"`，然後 `glab issue close <n>`，最後將上下文指針（gist + 連結）附加到地圖的 Decisions-so-far。
