# 外掛程式技能

預建的 AI 代理程式技能，透過 Muggle AI 自動化測試與開發工作流程。

## 本頁內容

| 章節 | 說明 |
| :------ | :---------- |
| [什麼是外掛程式技能？](#what-are-plugin-skills) | 總覽與安裝 |
| [可用技能](#available-skills) | 所有技能一覽 |
| [/muggle:do](#muggledo) | 品質有保障的開發工作流程 |
| [/muggle:test](#muggletest) | 由變更驅動的 E2E 路由 |
| [/muggle:test-feature-local](#muggletest-feature-local) | 快速的單一功能本機測試 |
| [/muggle:status](#mugglestatus) | 健康檢查 |
| [/muggle:repair](#mugglerepair) | 診斷並修復安裝 |
| [/muggle:upgrade](#muggleupgrade) | 更新至最新版本 |
| [更多技能](#more-skills) | 路由、準備、匯入、重新產生、PR、意見回饋、偏好設定 |
| [技能與直接呼叫 MCP 工具](#skills-vs-direct-mcp-tool-calls) | 兩種方式各自的適用時機 |
| [後續步驟](#next-steps) | 延伸閱讀 |

## 什麼是外掛程式技能？

外掛程式技能是結構化的工作流程，用來教 AI 助理如何執行複雜的多步驟任務。你不必手動逐一呼叫工具，只要呼叫一個技能，代理程式就會自動依照工作流程執行。

## 安裝

技能透過 Claude Code 適用的 Muggle AI 外掛程式提供：

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

安裝後，即可透過 `/muggle:*` 指令使用技能。

## 可用技能

| 技能 | 說明 | 觸發範例 |
| :---- | :---------- | :-------------- |
| `/muggle:do` | 品質有保障的開發工作流程：需求、撰寫程式碼、測試、以真實瀏覽器驗證的端對端（E2E）驗收測試，以及建立 PR | 「新增一個登出按鈕，並在開 PR 之前確認它能正常運作」 |
| `/muggle:test` | 由變更驅動的 E2E 路由——偵測程式碼變更、將變更對應到使用者旅程、在本機或遠端執行測試產生、將結果發布到儀表板並在瀏覽器中開啟，再於 PR 張貼 E2E 摘要 | 「測試我的變更，並把結果張貼到 PR」 |
| `/muggle:test-feature-local` | 針對 localhost 執行真實瀏覽器 E2E 驗收測試，驗證某項功能是否正常運作——註冊流程、結帳、表單驗證、UI 互動，或任何面向使用者的行為 | 「在 localhost:3000 上測試我的登入流程」 |
| `/muggle:status` | 檢查瀏覽器測試執行器（Electron）、MCP 伺服器與身分驗證的健康狀態 | 「檢查 muggle 狀態」 |
| `/muggle:repair` | 診斷並修復損壞的安裝 | 「修復我的 muggle 安裝」 |
| `/muggle:upgrade` | 更新至最新版本 | 「升級 muggle」 |
| `/muggle` | 指令路由，以及所有 Muggle 指令的選單 | 「muggle」 |
| `/muggle:test-prepare` | 測試前確認開發伺服器與相關服務都已啟動 | 「我的服務都啟動了嗎？」 |
| `/muggle:import` | 將現有的測試、PRD 或規格文件匯入 Muggle Test | 「匯入我的 Playwright 測試」 |
| `/muggle:regenerate-missing` | 為沒有腳本的測試案例批次重新產生腳本 | 「重新產生缺少的測試腳本」 |
| `/muggle:pr` | 將附有螢幕截圖的 E2E 視覺化導覽張貼到 PR | 「把視覺化導覽加到我的 PR」 |
| `/muggle:feedback` | 標記有誤的腳本或步驟，讓 Muggle 重新產生 | 「這次執行的結果不對」 |
| `/muggle:preferences` | 檢視、設定或重設 Muggle 測試偏好設定 | 「顯示我的 muggle 設定」 |

## /muggle:do

品質有保障的開發工作流程，帶領一項任務依序完成需求、撰寫程式碼、測試、以真實瀏覽器驗證的 E2E 驗收測試，以及建立 PR。AI 會撰寫功能的程式碼並執行單元測試，接著在真實瀏覽器中以桌面版與行動版檢視區對應用程式執行 E2E 測試，排查失敗原因，並開出附有佐證的 PR。

### 範例

```
/muggle:do "Add a logout button to the header"

REQUIREMENTS  -> Goal: Add logout button
IMPACT        -> frontend repo, src/components/Header.tsx
CODING        -> (writes code)
UNIT_TESTS    -> 12/12 pass
E2E           -> 3/3 test cases pass
OPEN_PRS      -> PR #42 opened
```

特色：

- 以工作階段為基礎，支援當機後復原
- 自動排查：分析失敗原因並回頭修正（最多 3 輪）
- 透過 `muggle-repos.json` 支援多個儲存庫
- PR 附有 E2E 結果與螢幕截圖


## /muggle:test

由變更驅動的 E2E 路由，會偵測程式碼變更、找出受影響的使用者旅程，並在本機（在 localhost 上使用 Electron 瀏覽器）或遠端（由 Muggle 雲端針對預覽/預備環境網址）執行測試產生。執行完成後，會將結果發布到 Muggle AI 儀表板並在你的瀏覽器中開啟，再於你的 PR 張貼附有測試案例連結的 E2E 摘要。

### 兩種模式

| 模式 | 適用時機 | 執行工具 |
| :--- | :---------- | :------------- |
| **本機** | 開發期間針對 `localhost` 進行測試 | `muggle-local-execute-test-generation` |
| **遠端** | 針對預覽或預備環境網址進行測試 | `muggle-remote-workflow-start-test-script-generation` |

此技能會在繼續之前詢問你要使用哪種模式。如果你的提示詞提到「localhost」或「local」，就會選擇本機模式。如果提到「staging」、「preview」或非 localhost 的網址，則會選擇遠端模式。

### 範例

```
/muggle:test

"Test my changes and post the results to the PR"

1. Intent: local or remote? -> "local, localhost:3000"
2. Changes detected: signup form refactor, profile page update
3. Auth: authenticated
4. Project: "My App" (matched)
5. Use cases: 2 existing, 1 new -> confirm mapping
6. Test cases: 4 total -> confirm scope
7. Launching browser test runner x4... (approve? y)
8. Results: 3/4 PASS, 1 FAIL
9. Published to Muggle dashboard (opened in browser)
10. E2E results posted to PR #42
```

### 測試執行後的動作

- **發布**：透過 `muggle-local-publish-test-script` 將每次執行發布到 Muggle AI 雲端
- **開啟**：在你的瀏覽器中開啟 Muggle AI 儀表板，方便你檢視逐步的螢幕截圖
- **張貼**：在 PR 張貼一則 E2E 結果留言，其中的通過/失敗表格裡，每個測試案例都連結到它在儀表板上的詳細資料頁面

### 何時使用 /muggle:test，何時使用 /muggle:test-feature-local

| 情境 | 建議的技能 |
| :------- | :---------------- |
| 測試受你的變更影響的所有使用者旅程 | `/muggle:test` |
| 在預備環境/預覽網址上測試 | `/muggle:test` |
| 將 E2E 結果張貼到 PR | `/muggle:test` |
| 在 localhost 上快速測試單一功能 | `/muggle:test-feature-local` |


## /muggle:test-feature-local

針對 localhost 執行真實瀏覽器 E2E 驗收測試，驗證某項功能是否正常運作——註冊流程、結帳、表單驗證、UI 互動，或任何面向使用者的行為。它會啟動瀏覽器來執行測試步驟並擷取螢幕截圖。

### 範例

```
/muggle:test-feature-local

"Test my login changes on localhost:3000"

1. Auth check
2. Found project: "My App"
3. Found 2 test cases - recommend replay
4. Launching browser test runner... (approve? y)
5. Results: 2/2 PASS
6. Publish to cloud? (y)
```

此技能會處理完整的工作流程：身分驗證、尋找或建立測試案例、在真實瀏覽器中執行、附上螢幕截圖回報結果，並可選擇將產生的腳本發布到雲端。

## /muggle:status

檢查你的 Muggle AI 安裝的健康狀態：

- Electron 瀏覽器測試執行器的版本與二進位檔完整性
- MCP 伺服器的回應狀況
- 身分驗證狀態與權杖到期時間

如果任何一項檢查失敗，請執行 `/muggle:repair`。

## /muggle:repair

自動診斷並修復損壞的元件：

- Electron 瀏覽器測試執行器遺失或損毀時重新下載
- 憑證過期時重新進行身分驗證
- 回報修復了哪些項目

## /muggle:upgrade

將所有 Muggle AI 元件更新至最新版本，並回報更新前後的版本。

## 更多技能

| 技能 | 功能 |
| :---- | :----------- |
| `/muggle` | 路由與選單——列出每個 Muggle 指令並分派執行。 |
| `/muggle:test-prepare` | 檢查測試所需的開發伺服器與相關服務是否正在執行，並可協助啟動尚未執行的服務。 |
| `/muggle:import` | 將現有測試匯入 Muggle Test——Playwright、Cypress、Gherkin `.feature` 檔案、PRD 或測試計畫文件。 |
| `/muggle:regenerate-missing` | 掃描專案中沒有有效腳本的測試案例，並透過雲端批次產生缺少的腳本。 |
| `/muggle:pr` | 為每個測試案例建立視覺化導覽（儀表板連結與步驟螢幕截圖），並張貼到 PR。 |
| `/muggle:pr-followup` | 監看一個尚未關閉的 PR 是否有新送出的審查，並交給 `/muggle:do` 處理。 |
| `/muggle:feedback` | 標記某個產生的腳本——或其中某個特定步驟——執行有誤，讓 Muggle 重新產生。也能列出及刪除意見回饋。 |
| `/muggle:preferences` | 檢視、設定或重設控制測試行為的偏好設定，例如自動登入。 |

## 技能與直接呼叫 MCP 工具

| 方式 | 適用時機 |
| :------- | :---------- |
| **技能** | 常見工作流程、多步驟任務、「直接幫我做出來」或「直接幫我測試」 |
| **直接呼叫工具** | 精細控制、特定操作、自訂工作流程 |

技能在底層呼叫的是同一套 100 多個 MCP 工具——它們負責自動協調整個流程。

## 後續步驟

- **[工具參考](tools-reference.md)**——各個工具的說明文件
- **[範例工作流程](examples.md)**——看看工具與技能的實際運用
- **[本機測試安裝與設定](setup.md)**——安裝與設定
