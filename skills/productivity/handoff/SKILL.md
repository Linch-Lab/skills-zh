---
name: handoff
description: 此 skill 禁止自動調用。將當前對話壓縮為交接文件，供另一個 agent 接手。
argument-hint: "下一個對話將用於什麼？"
---

撰寫一份交接文件，摘要當前對話，使全新 agent 能繼續工作。儲存到使用者 OS 的暫存目錄——非當前工作區。

文件中包含「建議 skills」區段，建議 agent 應調用的 skills。

不重複已在其他工件中捕捉的內容（規格、計畫、ADR、issue、commit、diff）。改以路徑或 URL 引用。

編輯任何敏感資訊，如 API 金鑰、密碼或個人識別資訊。

若使用者傳入參數，將其視為對下一個對話重點的描述，並據此調整文件。
