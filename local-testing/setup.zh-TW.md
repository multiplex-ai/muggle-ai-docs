# 本機測試安裝與設定

安裝並設定 Muggle Test，測試你的 localhost 應用程式。

## 本頁內容

| 章節 | 說明 |
| :------ | :---------- |
| [必要條件](#prerequisites) | 開始之前需要準備的項目 |
| [步驟 1：安裝](#step-1-install) | 安裝套件 |
| [步驟 2：設定你的 AI 助理](#step-2-configure-your-ai-assistant) | 各平台的 MCP 設定（Claude Code、Cursor、Antigravity） |
| [步驟 3：重新啟動你的 AI 助理](#step-3-restart-your-ai-assistant) | 載入新的設定 |
| [步驟 4：驗證連線](#step-4-verify-the-connection) | 確認可正常運作 |
| [執行你的第一個測試](#running-your-first-test) | 建立專案並進行測試 |
| [CLI 指令](#cli-commands) | 可用的命令列工具 |
| [設定選項](#configuration-options) | 環境變數與自訂設定 |
| [身分驗證](#authentication) | 登入以執行測試 |
| [資料儲存](#data-storage) | 本機資料的存放位置 |
| [疑難排解](#troubleshooting) | 常見問題與解決方法 |
| [後續步驟](#next-steps) | 延伸閱讀 |

## 必要條件

| 需求 | 詳細資訊 |
| :---------- | :------ |
| **Node.js** | 22 以上版本 |
| **MCP 用戶端** | Claude Code、Cursor、Antigravity，或任何相容 MCP 的助理 |
| **磁碟空間** | 套件約需 500MB（含內建的瀏覽器引擎） |

## 步驟 1：安裝

**Claude Code（建議）：**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

**npm（適用於 Cursor 或只使用 CLI 的情況）：**

```bash
npm install -g @muggleai/works@latest
```

安裝期間，套件會自動：
- 下載對應平台的瀏覽器引擎
- 設定 Cursor MCP（`~/.cursor/mcp.json`）
- 將技能同步到 `~/.cursor/skills/`

套件內容：

| 元件 | 用途 |
| :-------- | :------ |
| `muggle` | 整合式 MCP 伺服器與 CLI |
| 瀏覽器引擎 | 以 Electron 為基礎的瀏覽器自動化（依平台下載） |
| 本機工具 | 約 20 個工具，用於本機執行、檢視結果與發布 |
| 雲端工具 | 約 80 個工具，用於雲端 E2E 驗收測試與實體管理 |

### 驗證安裝

安裝完成後，請確認一切都已正確設定：

```bash
muggle doctor
```

這個指令會檢查：
- Node.js 版本
- 瀏覽器引擎是否已安裝
- 資料目錄狀態
- 身分驗證狀態

**預期輸出：**

```
Muggle Doctor
=============
✓ Data Directory: Found at ~/.muggle-ai
✓ Electron App: Installed (v1.0.0)
✗ Authentication: Not authenticated
  └─ Run 'muggle login' to authenticate
✓ Prompt Service URL: https://promptservice.muggle-ai.com
```

## 步驟 2：設定你的 AI 助理

### Cursor IDE

**不需要手動設定。** 透過 npm 安裝時，會自動將 muggle 伺服器加入 `~/.cursor/mcp.json`。安裝後重新啟動 Cursor 即可。

若要自訂（例如僅限本機模式），請編輯 `~/.cursor/mcp.json`：

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve", "--local"]
    }
  }
}
```

### Antigravity

編輯 `~/.gemini/antigravity/mcp_config.json`（或開啟 Agent 面板 → **MCP Servers** → **Manage MCP Servers** → **View raw config**）：

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"]
    }
  }
}
```

**僅用於本機測試**（不含雲端功能）：

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve", "--local"]
    }
  }
}
```

### Claude Desktop

編輯 Claude Desktop 的設定：

| 平台 | 設定檔位置 |
| :------- | :------------------- |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"]
    }
  }
}
```

## 步驟 3：重新啟動你的 AI 助理

重新啟動你的 MCP 用戶端（Cursor、Antigravity 或 Claude Desktop），以載入新的 MCP 伺服器設定。Claude Code 使用者可以略過此步驟。

## 步驟 4：驗證連線

| 平台 | 驗證方式 |
| :------- | :------------ |
| **Claude Code** | 執行 `/muggle:status` |
| **Cursor** | 對你的助理說：**「檢查 Muggle Test 的狀態」** |
| **Antigravity** | 對你的代理程式說：**「檢查 Muggle Test 的狀態」** |
| **Claude Desktop** | 對你的助理說：**「檢查 Muggle Test 的狀態」** |

你應該會看到一則表示 MCP 已就緒的回應：

```
Muggle Test Local Status
======================
Data Directory: ~/.muggle-ai
Sessions Directory: ~/.muggle-ai/sessions

Authentication
--------------
Authenticated: No
```

## 執行你的第一個測試

連線成功後，試著建立一個專案：

> 「幫我的應用程式建立一個測試專案，網址是 http://localhost:3000」

助理會：
1. 視需要先讓你登入，再於你的 Muggle Test 帳戶中建立專案
2. 回傳專案詳細資訊
3. 準備就緒，讓你新增使用者旅程與測試案例

## CLI 指令

`muggle` 指令提供以下實用工具：

| 指令 | 說明 |
| :------ | :---------- |
| `muggle serve` | 啟動 MCP 伺服器（預設：所有工具） |
| `muggle serve --local` | 僅啟用本機工具 |
| `muggle serve --e2e` | 僅啟用雲端 E2E 驗收工具 |
| `muggle setup` | 下載並安裝瀏覽器引擎 |
| `muggle setup --force` | 即使已安裝也強制重新下載 |
| `muggle doctor` | 檢查安裝並診斷問題 |
| `muggle login` | 向 Muggle AI 進行身分驗證 |
| `muggle logout` | 清除已儲存的憑證 |
| `muggle status` | 顯示身分驗證狀態 |

## 設定選項

### 環境變數

你可以在 MCP 設定中透過環境變數自訂行為：

| 變數 | 預設值 | 說明 |
| :------- | :------ | :---------- |
| `LOG_LEVEL` | `info` | 記錄的詳細程度（`debug`、`info`、`warn`、`error`） |
| `ELECTRON_APP_PATH` | （自動下載） | 瀏覽器引擎執行檔的自訂路徑 |
| `MCP_API_KEY` | - | 用於雲端功能的 API 金鑰 |
| `PROMPT_SERVICE_BASE_URL` | （正式環境） | 覆寫 Prompt Service 的網址 |
| `MUGGLE_LLM_*` | - | 以你自己的模型執行本機測試；請參閱[模型來源](local-testing/model-sources.md) |

**自訂設定範例：**

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "LOG_LEVEL": "debug",
        "MCP_API_KEY": "mai_sk_your_key_here"
      }
    }
  }
}
```

## 身分驗證

本機測試採用**雲端優先**架構：連接 MCP 伺服器不需要任何憑證，但**執行測試必須登入**。測試執行的是儲存在你 Muggle Test 帳戶中的測試案例，因此伺服器會在啟動瀏覽器之前先驗證你的身分。發布結果與使用雲端 E2E 功能同樣需要登入。

### 裝置碼流程

1. 對你的助理說：「登入 Muggle Test」
2. 你會收到一個網址和一組裝置碼，請在瀏覽器中開啟
3. 在瀏覽器中完成登入
4. 助理會自動輪詢，確認登入是否完成

**流程範例：**

> **你**：「登入 Muggle Test」
>
> **助理**：請完成身分驗證：
> - 開啟：`https://login.muggle-ai.com/activate?user_code=ABCD-1234`
> - 你登入後，我會確認你的身分驗證狀態。
>
> **你**：「好了，幫我確認是否已登入」
>
> **助理**：身分驗證成功！已登入為 user@example.com

身分驗證權杖會儲存在本機的 `~/.muggle-ai/auth.json`。

## 資料儲存

執行結果、偏好設定與憑證儲存在本機的 `~/.muggle-ai/`。專案、使用者旅程與測試案例則存放在你的 Muggle Test 雲端帳戶中，而不是磁碟上。

```
~/.muggle-ai/
├── auth.json                    # Authentication tokens
├── credentials.json             # API credentials
├── preferences.json             # Your testing preferences
├── electron-app/                # Browser automation engine
│   └── {version}/
└── sessions/                    # Local run results and screenshots
    └── {session_id}/
        ├── result.json
        └── screenshots/
```

## 疑難排解

### 工具沒有出現

| 檢查項目 | 解決方法 |
| :---- | :------- |
| 套件已安裝？ | 執行 `npm list -g @muggleai/works` |
| 設定正確？ | 檢查所用平台設定檔中的 JSON 語法 |
| 用戶端已重新啟動？ | 變更設定後，重新啟動 MCP 用戶端 |
| Node 版本？ | 確認已安裝 Node.js 22 以上版本（`node --version`） |

**各平台設定檔路徑：**

| 平台 | 設定檔 |
| :------- | :---------- |
| Claude Code | 由外掛程式系統管理 |
| Cursor | `~/.cursor/mcp.json` |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` |
| Claude Desktop（macOS） | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop（Windows） | `%APPDATA%\Claude\claude_desktop_config.json` |

### 「Electron-app not found」

瀏覽器引擎會在安裝期間自動下載。如果你看到這個錯誤：

| 檢查項目 | 解決方法 |
| :---- | :------- |
| 下載失敗？ | 執行 `muggle setup` 重新下載 |
| 網路問題？ | 檢查網際網路連線與 代理伺服器設定 |
| 自訂位置？ | 設定 `ELECTRON_APP_PATH` 環境變數 |
| 平台是否支援？ | 支援 macOS（arm64、x64）、Windows（x64）與 Linux（x64） |

### 「Download failed with status 404」

這表示找不到對應的發行資產。常見原因：

| 原因 | 解決方法 |
| :---- | :------- |
| 新版本尚未發布 | 查看[可用的發行版本](https://github.com/multiplex-ai/muggle-ai-works/releases) |
| 開發版本 | 本機開發時請手動建置（見下方說明） |

**從原始碼建置（適用於開發人員）：**

```bash
git clone https://github.com/multiplex-ai/muggle-ai-works
cd muggle-ai-works
npm install
npm run build
```

或在 MCP 設定中設定 `ELECTRON_APP_PATH` 環境變數。

### 身分驗證錯誤

| 錯誤 | 解決方法 |
| :---- | :------- |
| 「Device code expired」 | 裝置碼的有效期限為 15 分鐘。請重新登入。 |
| 「Authorization pending」 | 先在瀏覽器中完成登入，再檢查狀態。 |
| 權杖已過期 | 執行 `muggle login` 取得新的權杖。 |

如需更多疑難排解說明，請參閱[常見問題](troubleshooting/common-issues.md)。

## 後續步驟

- **[本機測試總覽](overview.md)**——了解整體架構
- **[可用工具](tools-reference.md)**——了解有哪些工具可用
- **[範例工作流程](examples.md)**——常見的測試模式
- **[代理程式技能](skills.md)**——預建的測試工作流程
