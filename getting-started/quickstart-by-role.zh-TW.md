# 依角色快速入門

選擇你的角色，看看用 Muggle Test 最快發揮效益的途徑。

## 快速導覽

| 角色 | 最適合 | 前往 |
| :--- | :------- | :------ |
| **開發人員** | 在本機建置、在 CI/CD 中測試、安心發布 | [開發人員指南](#developers) |
| **Vibe Coder** | 以提示詞驅動開發，搭配預覽網址測試 | [Vibe Coder 指南](#vibe-coders) |
| **產品團隊** | 以 PRD 為基礎產生測試，以及問題分級處理的工作流程 | [產品團隊指南](#product-team) |

**常用資源**：[調查失敗原因](#investigating-failures) | [管理已探索的流程](#managing-discovered-flows) | [檢閱結果](#reviewing-results) | [後續步驟](#next-steps)

---

## 開發人員

**你的工作流程**：開發功能 → 在本機測試 → 修正問題 → 安心發布

### 建議路徑

| 步驟 | 動作 | 指南 |
| :--- | :----- | :---- |
| 1 | 為你的 AI 助理安裝 Muggle Test Local | [本機測試安裝與設定](local-testing/setup.md) |
| 2 | 一邊寫程式，一邊測試你的 localhost 應用程式 | [本機測試範例](local-testing/examples.md) |
| 3 | 根據測試報告修正問題 | [調查失敗原因](#investigating-failures) |
| 4 | 為你的預備環境建立專案 | [執行你的第一批測試](quickstart-running-tests.md) |
| 5 | 整合到 CI/CD，進行自動化迴歸測試 | [CI/CD 整合](mcp/mcp-cicd-integration.md) |

### 主要工具

- **本機測試**：直接從 Cursor、Claude 或 VS Code 測試 localhost
- **MCP 整合**：讓你的 AI 助理在你寫程式時執行測試
- **CI/CD 掛鉤**：關鍵使用者旅程失敗時阻擋部署

---

## Vibe Coder

**你的工作流程**：用提示詞描述功能 → 在雲端驗證 → 快速修正 → 安心發布

### 建議路徑

| 步驟 | 動作 | 指南 |
| :--- | :----- | :---- |
| 1 | 將 Muggle Test 連接到你的 AI 助理 | [MCP 快速入門](mcp-quickstart.md) |
| 2 | 請你的 AI 測試你的預覽網址 | [本機測試範例](local-testing/examples.md) |
| 3 | 檢閱報告並修正問題 | [調查失敗原因](#investigating-failures) |
| 4 | 重新測試，直到檢查通過 | 重複步驟 2 |

### 主要工具

- **MCP 技能**：只要說「測試我的應用程式」，其餘的就交給你的 AI 處理
- **預覽網址測試**：支援 Vercel、Netlify、Cloudflare 預覽
- **快速回饋循環**：幾分鐘內就有結果，不必等上幾小時

### 範例提示詞

```
"Test my Vercel preview at https://my-app-abc123.vercel.app"

"Run Muggle Test on my staging site and tell me what's broken"

"Check if the signup flow works on localhost:3000"
```

---

## 產品團隊

**你的工作流程**：匯入需求 → 產生測試 → 檢閱發現的問題 → 分級處理修正

### 建議路徑

| 步驟 | 動作 | 指南 |
| :--- | :----- | :---- |
| 1 | 為你的預備環境建立專案 | [執行你的第一批測試](quickstart-running-tests.md) |
| 2 | 上傳你的 PRD 或需求文件 | [上傳 PRD 檔案](concepts/prd-upload.md) |
| 3 | 檢閱並核准產生的使用者旅程 | [管理已探索的流程](#managing-discovered-flows) |
| 4 | 執行測試並檢閱報告 | [檢閱結果](#reviewing-results) |
| 5 | 依嚴重程度分級處理問題，並指派給開發人員 | [調查失敗原因](#investigating-failures) |

### 主要工具

- **PRD 上傳**：從現有文件中擷取測試情境
- **AI 產生的使用者旅程**：用日常語言描述你想測試的內容
- **可分享的報告**：將發現的問題匯出到 Jira、Linear 或 Slack

### 可上傳的內容

| 格式 | 最適合 |
| :----- | :------- |
| PDF | 產品規格、設計文件 |
| DOCX | 需求文件、使用者故事 |
| TXT | 純文字規格 |

---

## 通用參考

### 調查失敗原因

針對每個失敗的測試，你可以看到：

| 資訊 | 用途 |
| :---------- | :------ |
| **動作** | 失敗的確切步驟 |
| **螢幕截圖** | 失敗當下的畫面狀態 |
| **錯誤訊息** | 用於偵錯的技術細節 |
| **頁面網址** | 失敗發生的位置 |

### 管理已探索的流程

| 動作 | 使用時機 |
| :----- | :---------- |
| **重新命名** | 配合你所屬領域的用語 |
| **停用** | 排除你不在意的流程 |
| **設為優先** | 標記關鍵業務流程 |

### 檢閱結果

使用篩選條件聚焦在重要的內容：

| 篩選條件 | 用途 |
| :----- | :------- |
| **狀態** | 只顯示通過或失敗的測試 |
| **流程名稱** | 聚焦在特定的使用者旅程 |
| **嚴重程度** | 優先處理影響重大的問題 |

---

## 後續步驟

| 目標 | 資源 |
| :--- | :------- |
| 了解 Muggle Test 的運作方式 | [Muggle Test 的運作方式](concepts/how-muggle-test-works.md) |
| 建立自動化測試 | [CI/CD 整合](mcp/mcp-cicd-integration.md) |
| 了解配額與定價 | [訂閱方案](billing/subscription-plans.md) |
| 解決問題 | [常見問題](troubleshooting/common-issues.md) |
