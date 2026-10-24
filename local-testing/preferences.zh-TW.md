# 偏好設定

偏好設定控制 Muggle Test 在測試期間的行為——例如是否沿用你上次使用的專案、是否顯示瀏覽器視窗、是否自動發布結果等。這些偏好設定由 `@muggleai/works` 的技能與工具讀取，並儲存在本機的 `~/.muggle-ai/preferences.json`。

## 設定值

詢問你的 AI 助理，或使用 `/muggle:preferences` 技能：

> 「顯示我的 muggle 偏好設定」
>
> 「將 autoLogin 設為 always」
>
> 「重設 muggle 偏好設定」

大多數偏好設定接受以下三個值之一：

| 值 | 行為 |
| :----- | :------- |
| `ask` | 每次都詢問你（預設） |
| `always` | 自動執行，不詢問 |
| `never` | 自動略過，不詢問 |

少數偏好設定使用不同的值組合，已在表格中註明。

## 可用的偏好設定

| 偏好設定 | 值 | 預設值 | 控制項目 |
| :--------- | :----- | :------ | :------- |
| `autoLogin` | always / ask / never | ask | 工具需要身分驗證時，直接沿用已儲存的憑證，不另行詢問 |
| `autoSelectProject` | always / ask / never | ask | 不詢問，直接沿用此儲存庫上次使用的專案 |
| `autoSelectLocalHost` | always / ask / never | ask | 沿用上一次本機執行所用的開發伺服器網址 |
| `showElectronBrowser` | always / ask / never | ask | 在本機測試期間顯示 Electron 瀏覽器視窗 |
| `openTestResultsAfterRun` | always / ask / never | ask | 本機執行結束後，在儀表板上開啟該次執行的結果頁面 |
| `defaultExecutionMode` | local / remote / ask | ask | 當技能同時支援兩種執行模式時，預設使用哪一種 |
| `autoPublishLocalResults` | always / ask / never | ask | 將本機結果上傳至雲端，方便團隊檢視 |
| `suggestRelatedUseCases` | always / ask / never | ask | 建立或執行使用者旅程後，建議相關的使用者旅程 |
| `suggestRelatedTestCases` | always / ask / never | ask | 建立或執行測試案例後，建議相關的測試案例 |
| `autoDetectChanges` | always / ask / never | ask | 掃描本機的 git 變更，並將其對應到受影響的測試案例 |
| `postPRVisualWalkthrough` | always / ask / never | ask | 測試結束後，在 PR 上張貼含螢幕截圖的視覺化導覽留言 |
| `autoCreatePR` | always / ask / never | ask | 不詢問，直接推送分支並建立 PR |
| `checkForUpdates` | always / ask / never | ask | 在工作階段開始時檢查是否有較新的 Muggle Test 版本 |
| `verboseOutput` | always / ask / never | ask | 在技能與工具執行期間顯示詳細的進度記錄 |
| `autoUseWorktree` | always / ask / never | ask | 建立 git worktree 來隔離較複雜的工作 |
| `autoRebase` | always / ask / never | ask | 在啟動開發伺服器或執行測試之前，先 rebase 到預設分支 |
| `autoCleanup` | always / ask / never | ask | PR 合併後，移除 worktree、刪除分支並清除產出物 |
| `autoE2ETest` | always / ask | always | 在每個 `/muggle:do` 週期結束時執行 E2E 驗收測試 |

## 重設

> 「重設我的 muggle 偏好設定」

這會將所有偏好設定恢復為預設值——大多數為 `ask`，`autoE2ETest` 則為 `always`。

## 後續步驟

| 目標 | 資源 |
| :--- | :------- |
| 查看會讀取這些偏好設定的技能 | [外掛程式技能](local-testing/skills.md) |
| 設定本機測試 | [本機測試安裝與設定](local-testing/setup.md) |
| 使用你自己的模型執行本機測試（`llmEnv` 區塊） | [模型來源](local-testing/model-sources.md) |
