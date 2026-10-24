# 本機測試工具參考

`@muggleai/works` 套件中本機測試工具的完整參考。

> **架構說明：** 本機測試採用**雲端優先**的做法：
> - **`muggle-remote-*` 工具**：身分驗證與所有實體管理（專案、使用者旅程、測試案例、密鑰）
> - **`muggle-local-*` 工具**：僅用於本機執行、檢視結果與發布

## 本頁內容

| 章節 | 說明 |
| :------ | :---------- |
| [工具命名空間](#tool-namespaces) | 遠端與本機工具前綴 |
| [本機工具類別](#local-tool-categories) | 狀態、執行、結果、腳本、發布 |
| [身分驗證工具](#authentication-tools-muggle-remote-auth-) | 登入、登出、狀態、輪詢 |
| [遠端資料工具](#remote-data-tools-muggle-remote-) | 取得雲端實體 |
| [狀態工具](#status-tools) | 檢查服務狀態與工作階段 |
| [執行工具](#execution-tools) | 產生與重播測試腳本 |
| [執行結果工具](#run-result-tools) | 檢視執行結果 |
| [測試腳本工具](#test-script-tools) | 檢視本機產生的腳本 |
| [發布工具](#publishing-tools) | 將腳本上傳至雲端 |
| [工作流程：使用雲端實體進行本機測試](#workflow-local-testing-with-cloud-entities) | 建議的端對端工作流程 |
| [工具選擇指南](#tool-selection-guide) | 常見工作速查 |
| [後續步驟](#next-steps) | 延伸閱讀 |

## 工具命名空間

| 命名空間 | 用途 | 範例 |
| :-------- | :------ | :------- |
| `muggle-remote-auth-*` | 身分驗證 | `muggle-remote-auth-login`、`muggle-remote-auth-status` |
| `muggle-remote-*` | 雲端實體管理 | `muggle-remote-project-create`、`muggle-remote-test-case-get`、`muggle-remote-action-script-get` |
| `muggle-local-*` | 本機執行與結果 | `muggle-local-execute-test-generation` |

## 本機工具類別

| 類別 | 工具 | 用途 |
| :------- | :---- | :------ |
| **狀態** | `muggle-local-check-status`、`muggle-local-list-sessions` | 檢查本機服務狀態 |
| **執行** | `muggle-local-execute-test-generation`、`muggle-local-execute-replay` | 執行測試產生與重播 |
| **結果** | `muggle-local-run-result-list`、`muggle-local-run-result-get` | 檢視執行結果 |
| **腳本** | `muggle-local-test-script-list`、`muggle-local-test-script-get` | 檢視本機產生的腳本 |
| **發布** | `muggle-local-publish-test-script` | 將產生的腳本上傳至雲端 |
| **控制** | `muggle-local-cancel-execution` | 取消進行中的執行作業 |

---

## 身分驗證工具（muggle-remote-auth-*）

身分驗證工具屬於 `muggle-remote-*` 命名空間，因為它們需要與雲端互動。

### muggle-remote-auth-status

檢查目前的身分驗證狀態。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| （無） | — | — | — |

**回傳：**
- `authenticated` - 是否已登入
- `email` - 使用者電子郵件（已通過身分驗證時）
- `expiresAt` - Token 到期時間

---

### muggle-remote-auth-login

啟動裝置碼（Device Code）身分驗證流程。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `waitForCompletion` | boolean | 否 | 等待在瀏覽器中完成登入（預設：true） |
| `timeoutMs` | number | 否 | 逾時時間，單位為毫秒（預設：120000） |

**回傳：**
- `verificationUri` - 要在瀏覽器中開啟的網址
- `userCode` - 要輸入的使用者碼
- `deviceCode` - 用於輪詢的裝置碼（搭配 `muggle-remote-auth-poll` 使用）

**範例提示詞：**
> 「登入 Muggle Test」

---

### muggle-remote-auth-poll

輪詢裝置碼身分驗證是否完成。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `deviceCode` | string | 否 | 來自 `muggle-remote-auth-login` 的裝置碼 |

**回傳：**
- `status` - `complete`、`pending`、`expired` 或 `error`
- `email` - 使用者電子郵件（完成時）

---

### muggle-remote-auth-logout

清除已儲存的憑證。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| （無） | — | — | — |

---

## 遠端資料工具（muggle-remote-*）

用於取得雲端實體的遠端工具，通常會在本機執行前使用。

### muggle-remote-action-script-get

依 ID 取得完整的操作腳本內容。重播時必須使用——以測試腳本中的 `actionScriptId` 取得含元素標籤的完整可執行步驟。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `actionScriptId` | string | 是 | 來自 `testScript.actionScriptId` 的操作腳本 ID |

**回傳：** 操作步驟陣列，包含重播所需的元素標籤。

**用法：** 在 `muggle-remote-test-script-get` 之後呼叫此工具取得腳本內容，再執行 `muggle-local-execute-replay`。

---

## 狀態工具

### muggle-local-check-status

檢查 Muggle Test 本機服務的狀態。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| （無） | — | — | — |

**回傳：**
- `dataDirectory` - 本機資料儲存路徑
- `sessionsDirectory` - 工作階段儲存路徑
- `authenticated` - 是否已通過身分驗證

---

### muggle-local-list-sessions

列出所有已儲存的測試工作階段。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `limit` | number | 否 | 回傳的工作階段數量上限（預設：10） |

---

## 執行工具

### muggle-local-execute-test-generation

啟動真實瀏覽器存取你的 Web 應用程式，產生 E2E 驗收測試腳本。瀏覽器會在你的應用程式中導覽各頁面，執行測試案例中的步驟（例如註冊、填寫表單、點選完成各個流程），並產生附螢幕截圖、可重播的測試腳本。你可以用它為任何使用者流程建立新的瀏覽器測試。

**重要：** 請先呼叫 `muggle-remote-test-case-get` 取得測試案例詳細資料，再將其傳入此工具。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `testCase` | object | 是 | 來自 `muggle-remote-test-case-get` 的測試案例詳細資料 |
| `localUrl` | string | 是 | 要測試的本機網址（例如 `http://localhost:3000`） |
| `approveElectronAppLaunch` | boolean | 是 | 必須為 `true` 才能繼續 |
| `timeoutMs` | number | 否 | 逾時時間，單位為毫秒（預設：300000） |

**testCase 物件：**
- `id` - 雲端測試案例 ID
- `title` - 測試案例標題
- `goal` - 測試要驗證的內容
- `expectedResult` - 預期結果
- `precondition` - 前置條件（選填）
- `instructions` - 逐步指示（選填）

**執行過程：**
1. 啟動瀏覽器引擎
2. 導覽至 `localUrl`
3. AI 根據測試案例探索並記錄操作
4. 腳本儲存在本機

**範例工作流程：**
```
1. muggle-remote-test-case-get(testCaseId) → returns test case object
2. muggle-local-execute-test-generation({ testCase: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-execute-replay

在真實瀏覽器中重播現有的 E2E 驗收測試腳本，確認你的應用程式仍正常運作——適合在程式碼變更後進行迴歸測試。瀏覽器會執行每個已儲存的步驟並擷取螢幕截圖，讓你看到實際發生的情況。

**重要：** 重播前需要先進行兩次獨立的取得呼叫：
1. `muggle-remote-test-script-get` 取得腳本中繼資料（包含 `actionScriptId`）
2. `muggle-remote-action-script-get` 取得完整的操作腳本內容

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `testScript` | object | 是 | 來自 `muggle-remote-test-script-get` 的測試腳本中繼資料 |
| `actionScript` | array | 是 | 來自 `muggle-remote-action-script-get` 的操作腳本步驟 |
| `localUrl` | string | 是 | 要測試的本機網址 |
| `approveElectronAppLaunch` | boolean | 是 | 必須為 `true` 才能繼續 |
| `timeoutMs` | number | 否 | 逾時時間，單位為毫秒（預設：180000） |
| `showUi` | boolean | 否 | 執行期間顯示瀏覽器介面（預設：false，以無頭模式執行） |

**testScript 物件：**
- `id` - 雲端測試腳本 ID
- `name` - 腳本名稱
- `testCaseId` - 所屬測試案例 ID
- `actionScriptId` - 用於取得操作腳本內容的 ID

**網址改寫：**
- 操作腳本中的原始雲端網址會自動替換為 `localUrl`

**範例工作流程：**
```
1. muggle-remote-test-script-get(testScriptId) → returns testScript with actionScriptId
2. muggle-remote-action-script-get(testScript.actionScriptId) → returns actionScript array
3. muggle-local-execute-replay({ testScript, actionScript, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-cancel-execution

取消進行中的執行作業。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 要取消的執行 ID |

---

## 執行結果工具

### muggle-local-run-result-list

列出執行結果。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | 否 | 依雲端測試案例 ID 篩選 |
| `limit` | number | 否 | 結果數量上限（預設：20） |

---

### muggle-local-run-result-get

取得執行結果詳細資料，包括螢幕截圖。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 執行結果 ID |

**回傳：**
- `id` - 執行 ID
- `runType` - `generation` 或 `replay`
- `status` - 通過/失敗結果
- `executionTimeMs` - 耗時
- `testScriptId` - 關聯的腳本（若已產生）
- `errorMessage` - 錯誤詳細資料（若失敗）

---

## 測試腳本工具

### muggle-local-test-script-list

列出本機產生的測試腳本。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | 否 | 依雲端測試案例 ID 篩選 |

---

### muggle-local-test-script-get

取得測試腳本詳細資料，包括操作腳本步驟。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `testScriptId` | string | 是 | 本機測試腳本 ID |

---

## 發布工具

### muggle-local-publish-test-script

將本機產生的測試腳本上傳至雲端。

| 參數 | 類型 | 必填 | 說明 |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | 是 | 產生測試時取得的執行結果 ID |
| `cloudTestCaseId` | string | 是 | 發布目標的雲端測試案例 ID |

**回傳：** 一個 `viewUrl`，可在 MuggleTest 儀表板上開啟已發布的測試腳本。使用 `open "<viewUrl>"`（macOS）或同等指令，在瀏覽器中檢視腳本詳細資料。

**前提：** 透過 `muggle-remote-auth-login` 完成身分驗證

---

## 工作流程：使用雲端實體進行本機測試

建議的工作流程使用雲端工具（`muggle-remote-*`）管理實體，並使用本機工具（`muggle-local-*`）執行：

```
┌─────────────────────────────────────────────────────────────┐
│                     Entity Management                        │
│                  (muggle-remote-* tools)                     │
├─────────────────────────────────────────────────────────────┤
│  muggle-remote-auth-login       → Authenticate               │
│  muggle-remote-project-list     → Find/create project        │
│  muggle-remote-use-case-list    → Find/create use case       │
│  muggle-remote-test-case-list   → Find/create test case      │
│  muggle-remote-test-case-get    → Fetch test case details    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     Local Execution                          │
│                 (muggle-local-* tools)                       │
├─────────────────────────────────────────────────────────────┤
│  muggle-local-execute-test-generation → Generate locally     │
│  muggle-local-run-result-get          → View results         │
│  muggle-local-publish-test-script     → Publish to cloud     │
└─────────────────────────────────────────────────────────────┘
```

### 範例：在 localhost:3000 上測試登入功能

```
1. muggle-remote-auth-status                         # Check auth
2. muggle-remote-auth-login                          # Login if needed
3. muggle-remote-project-list                        # Find project
4. muggle-remote-use-case-list(projectId)            # Find login use case
5. muggle-remote-test-case-list-by-use-case(ucId)    # Find login test case
6. muggle-remote-test-case-get(testCaseId)           # Get full details
7. [Ask user for approval]
8. muggle-local-execute-test-generation({
     testCase: { ...from step 6... },
     localUrl: "http://localhost:3000",
     approveElectronAppLaunch: true
   })
9. muggle-local-run-result-get(runId)           # View results
10. muggle-local-publish-test-script(runId, testCaseId)  # Publish
```

---

## 工具選擇指南

### 測試執行

| 你想要…… | 使用此流程 |
| :------------- | :------------ |
| 產生測試腳本 | `muggle-remote-test-case-get` → `muggle-local-execute-test-generation` |
| 重播現有腳本 | `muggle-remote-test-script-get` → `muggle-local-execute-replay` |
| 停止執行中的測試 | `muggle-local-cancel-execution` |
| 檢視測試結果 | `muggle-local-run-result-get` |

### 實體管理（使用 muggle-remote-* 工具）

| 你想要…… | 使用此工具 |
| :------------- | :------------ |
| 建立/列出專案 | `muggle-remote-project-create`、`muggle-remote-project-list` |
| 建立/列出使用者旅程 | `muggle-remote-use-case-create-from-prompts`、`muggle-remote-use-case-list` |
| 建立/列出測試案例 | `muggle-remote-test-case-create`、`muggle-remote-test-case-list` |
| 取得測試案例詳細資料 | `muggle-remote-test-case-get` |
| 取得測試腳本詳細資料 | `muggle-remote-test-script-get` |

### 身分驗證

| 你想要…… | 使用此工具 |
| :------------- | :------------ |
| 檢查身分驗證狀態 | `muggle-remote-auth-status` |
| 登入 | `muggle-remote-auth-login` |
| 輪詢登入是否完成 | `muggle-remote-auth-poll` |
| 登出 | `muggle-remote-auth-logout` |

---

## 後續步驟

- **[代理程式技能](skills.md)**——自動完成多步驟測試的預建工作流程
- **[範例工作流程](examples.md)**——看看這些工具的實際運用
- **[本機測試安裝與設定](setup.md)**——安裝與設定
- **[本機測試總覽](overview.md)**——了解架構
- **[雲端 E2E 驗收工具](mcp/mcp-api-reference.md)**——muggle-remote-* 工具的完整參考
