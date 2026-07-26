---
name: setup-matt-pocock-skills
description: 此 skill 禁止自動調用。為本 repo 設定工程類 skills 所需的基礎設施——議題追蹤系統、分類標籤用語、領域文件佈局。在其他工程類 skills 首次使用前執行一次。
---

# 設定 Matt Pocock Skills

搭建工程類 skills 依賴的 per-repo 設定：

- **議題追蹤系統**——issue 存放的位置（預設為 GitHub；也原生支援本地 markdown）
- **分類標籤**——五種標準分類角色對應的標籤字串
- **領域文件**——`CONTEXT.md` 和 ADR 的存放位置，以及讀取規則

這是一個對話引導型 skill，不是確定性腳本。探索、呈現發現、向使用者確認、然後寫入。

## 流程

### 1. 探索

查看當前 repo 以了解其起始狀態。讀取已存在的內容，不做假設：

- `git remote -v` 和 `.git/config`——這是 GitHub repo 嗎？哪一個？
- repo 根目錄的 `AGENTS.md` 和 `CLAUDE.md`——是否存在？其中是否已有 `## Agent skills` 段落？
- repo 根目錄的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 目錄及所有 `src/*/docs/adr/` 目錄
- `docs/agents/`——此 skill 先前的輸出是否已存在？
- `.scratch/`——表示本地 markdown 議題追蹤慣例已被使用
- `triage` skill 是否已安裝？（與此 skill 同層的 `triage` skill 目錄，或可用 skills 清單中有 `triage`）這決定 Section B 是否執行
- Monorepo 信號——`pnpm-workspace.yaml`、`package.json` 中的 `workspaces` 欄位、或包含自行 `src/` 的 `packages/*`。僅在真正的多套件 repo 中出現；不存在即為單一情境，這是絕大多數 repo 的情況

### 2. 呈現發現並提問

摘要已存在和缺失的內容。然後依序處理各節——一節一個答案，然後下一節。

每個節以建議答案開頭，讓使用者一個字就能接受。僅在選項有真正分支時提供一行說明；若探索已解決該節則完全跳過（如 `triage` 未安裝時跳過 Section B、無 monorepo 時跳過 Section C）。

**Section A——議題追蹤系統。**

> 說明：「議題追蹤系統」是本 repo 存放 issue 的位置。`to-tickets`、`triage`、`to-spec`、`qa` 等 skills 會從中讀取和寫入——它們需要知道該呼叫 `gh issue create`、在 `.scratch/` 下寫入 markdown 檔案、還是遵循你描述的其他工作流程。選擇你實際在此 repo 中追蹤工作的位置。

預設立場：這些 skills 是為 GitHub 設計的。若 `git remote` 指向 GitHub，提議 GitHub。若 `git remote` 指向 GitLab（`gitlab.com` 或自託管主機），提議 GitLab。否則（或使用者偏好），提供以下選項：

- **GitHub**——issue 存放在 repo 的 GitHub Issues 中（使用 `gh` CLI）
- **GitLab**——issue 存放在 repo 的 GitLab Issues 中（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown**——issue 以檔案形式存放在 `.scratch/<feature>/` 下（適合個人專案或無 remote 的 repo）
- **其他**（Jira、Linear 等）——請使用者用一段話描述工作流程；skill 將其記錄為自由文字

將選擇記錄在 `docs/agents/issue-tracker.md`。GitHub 和 GitLab 模板包含「PRs as a request surface」標誌，預設為**關閉**——維持關閉，不要提起；若使用者想讓外部 PR 進入分類佇列，之後可自行在檔案中打開。

**Section B——分類標籤用語。** 若 `triage` skill 未安裝（探索已告知），完全跳過此節——未安裝的 skill 不需要標籤。

若已安裝，只問一個問題：

> 是否保留預設分類標籤？（建議：**是**）

預設為五種標準角色，每個標籤字串等於其名稱：`needs-triage`、`needs-info`、`ready-for-agent`、`ready-for-human`、`wontfix`。選擇**是**則直接寫入。僅在使用者說不時——通常是因為其追蹤系統已使用其他名稱（如用 `bug:triage` 取代 `needs-triage`）——收集覆蓋值，讓 `triage` 套用現有標籤而非建立重複項。

**Section C——領域文件。** 預設為**單一情境**——repo 根目錄一個 `CONTEXT.md` + `docs/adr/`。這適用於幾乎所有 repo，直接寫入不需提問。

僅在探索發現 monorepo 信號時，提供**多重情境**選項——根目錄的 `CONTEXT-MAP.md` 指向各情境的 `CONTEXT.md` 檔案。然後確認使用者要哪種佈局。

### 3. 確認並編輯

向使用者展示以下內容的草稿：

- 要加入 `CLAUDE.md` / `AGENTS.md` 的 `## Agent skills` 區塊（選擇規則見步驟 4）
- `docs/agents/issue-tracker.md`、`docs/agents/domain.md`、`docs/agents/triage-labels.md` 的內容（最後一項僅在 `triage` 已安裝時）

讓使用者編輯後再寫入。

### 4. 寫入

**選擇要編輯的檔案：**

- 若 `CLAUDE.md` 存在，編輯它
- 否則若 `AGENTS.md` 存在，編輯它
- 若兩者都不存在，詢問使用者要建立哪一個——不要自己選

絕不在 `CLAUDE.md` 已存在時建立 `AGENTS.md`（反之亦然）——永遠編輯已存在的那個。

若所選檔案中已有 `## Agent skills` 區塊，在原位更新其內容，不要附加重複項。不要覆蓋使用者對周圍段落的編輯。

區塊內容：

```markdown
## Agent skills

### Issue tracker

[一行摘要：issue 追蹤的位置]。詳見 `docs/agents/issue-tracker.md`。

### Triage labels

[一行摘要：標籤用語]。詳見 `docs/agents/triage-labels.md`。

### Domain docs

[一行摘要：佈局——「單一情境」或「多重情境」]。詳見 `docs/agents/domain.md`。
```

僅在 `triage` 已安裝且 Section B 執行時，才包含 `### Triage labels` 子區塊並寫入 `docs/agents/triage-labels.md`。否則兩者皆省略。

接著使用此 skill 目錄中的種子模板作為起點寫入文件：

- [issue-tracker-github.md](./issue-tracker-github.md)——GitHub 議題追蹤系統
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md)——GitLab 議題追蹤系統
- [issue-tracker-local.md](./issue-tracker-local.md)——本地 markdown 議題追蹤系統
- [triage-labels.md](./triage-labels.md)——標籤對應（僅在 `triage` 已安裝時）
- [domain.md](./domain.md)——領域文件讀取規則 + 佈局

對於「其他」議題追蹤系統，根據使用者描述從頭撰寫 `docs/agents/issue-tracker.md`。

### 5. 完成

告知使用者設定已完成，以及哪些工程類 skills 現在會讀取這些檔案。提醒他們之後可直接編輯 `docs/agents/*.md`——只有切換議題追蹤系統或想從頭開始時才需要重新執行此 skill。
