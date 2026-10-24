# MCP 安裝

Muggle Test MCP 套件只需安裝一次，就能透過單一整合式伺服器同時支援**雲端 E2E 驗收測試**與**本機 E2E 驗收測試**。

## 本頁內容

| 章節 | 說明 |
| :------ | :---------- |
| [套件總覽](#package-overview) | 套件內含的項目 |
| [快速安裝](#quick-install) | 為 Claude Code、Cursor 或 Antigravity 安裝 |
| [部署選項](#deployment-options) | 託管閘道與本機安裝比較 |
| [用戶端設定](#client-configuration) | 各平台的 MCP 設定方式 |
| [工具選擇](#tool-selection) | 使用旗標限制工具範圍 |
| [CLI 指令](#cli-commands) | 可用的命令列工具 |
| [環境變數](#environment-variables) | 自訂行為 |
| [身分驗證](#authentication) | 裝置碼流程與 API 金鑰 |
| [驗證安裝](#verifying-installation) | 確認一切正常運作 |
| [架構](#architecture) | 各元件如何搭配運作 |
| [資料儲存](#data-storage) | 本機資料的存放位置 |
| [疑難排解](#troubleshooting) | 常見問題與解決方法 |
| [後續步驟](#next-steps) | 延伸閱讀 |

## 套件總覽

`@muggleai/works` 套件整合了以下功能：

| 功能 | 說明 |
| :------ | :---------- |
| **雲端 E2E 驗收工具** | 測試預覽、預備與正式環境 |
| **本機測試工具** | 透過瀏覽器自動化測試 localhost 應用程式 |
| **整合式 CLI** | 單一命令列介面涵蓋所有功能 |
| **共用身分驗證** | 登入一次即可同時使用本機與雲端功能 |

---

## 快速安裝

**Claude Code（建議）：**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

這會透過外掛程式生命週期提供技能、MCP 伺服器與掛鉤。不需要手動複製檔案。

**npm（適用於 Cursor 或僅使用 CLI 的情境）：**

```bash
npm install -g @muggleai/works@latest
```

這會自動安裝並設定所有項目：
- MCP 伺服器（`muggle serve`）
- 瀏覽器自動化引擎（自動下載）
- 用於診斷與身分驗證的 CLI 工具
- **Cursor MCP 設定**（`~/.cursor/mcp.json`，自動寫入）

---

## 部署選項

| 選項 | 適用情境 | 設定方式 |
| :----- | :------- | :---- |
| **託管閘道** | 快速上手、僅雲端測試 | 只需設定你的 MCP 用戶端 |
| **本機安裝** | 本機測試、更低延遲、隱私保護 | 安裝 npm 套件 |

### 選項 1：託管閘道（僅限雲端測試）

使用 Muggle AI 託管的 MCP 閘道測試遠端網址。不需要安裝。

**端點：** `https://mcp.muggle-ai.com/mcp`

**傳輸方式：** Streamable HTTP

**身分驗證：** 透過 `x-api-key` 標頭傳遞 API 金鑰

```json
{
  "mcpServers": {
    "muggle": {
      "url": "https://mcp.muggle-ai.com/mcp",
      "headers": {
        "x-api-key": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

### 選項 2：本機安裝（建議）

在本機安裝後，即可透過單一 MCP 伺服器同時進行**本機與雲端 E2E 驗收測試**。

#### 優點

| 優點 | 說明 |
| :------ | :---------- |
| **本機測試** | 測試 localhost 與區域網路網址 |
| **更低延遲** | 不需經過託管閘道的網路往返 |
| **隱私保護** | 憑證保留在你的電腦上 |
| **整合式體驗** | 一個伺服器滿足所有測試需求 |

#### 必要條件

| 需求 | 版本 |
| :---------- | :------ |
| Node.js | 22+ |
| npm | 10+ |

#### 安裝

```bash
npm install -g @muggleai/works@latest
```

安裝過程中，系統會自動為你的平台下載瀏覽器自動化引擎。

#### 驗證安裝

```bash
muggle doctor
```

此指令會檢查：
- Node.js 版本
- 瀏覽器引擎安裝狀態
- 資料目錄狀態
- 身分驗證狀態

---

## 用戶端設定

### Claude Code

不需要手動設定——透過外掛程式系統安裝：

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

### Cursor

`npm install -g @muggleai/works` 會自動將 muggle 伺服器加入 `~/.cursor/mcp.json`。**不需要手動設定**——安裝完成後重新啟動 Cursor 即可。

驗證方式：執行 `muggle doctor`，確認「Cursor MCP Config」顯示為通過。

**選填：新增 API 金鑰以使用雲端功能**

編輯 `~/.cursor/mcp.json` 以新增環境變數：

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
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

**使用 API 金鑰（用於雲端功能）：**

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

### Claude Desktop

編輯你的 Claude Desktop 設定：

| 平台 | 路徑 |
| :------- | :--- |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

---

## 工具選擇

伺服器預設會啟用**所有工具**（本機與雲端）。使用旗標可限制範圍：

| 指令 | 啟用的工具 |
| :------ | :------------ |
| `muggle serve` | 所有工具（本機 + 雲端） |
| `muggle serve --local` | 僅本機測試 |
| `muggle serve --e2e` | 僅雲端 E2E 驗收工具 |

**範例：僅本機的設定：**

```json
{
  "mcpServers": {
    "muggle-test-local": {
      "command": "muggle",
      "args": ["serve", "--local"]
    }
  }
}
```

---

## CLI 指令

| 指令 | 說明 |
| :------ | :---------- |
| `muggle serve` | 啟動 MCP 伺服器 |
| `muggle setup` | 下載/重新安裝瀏覽器引擎 |
| `muggle doctor` | 檢查安裝並診斷問題 |
| `muggle login` | 向 Muggle AI 進行身分驗證 |
| `muggle logout` | 清除已儲存的憑證 |
| `muggle status` | 顯示身分驗證狀態 |
| `muggle --version` | 顯示套件版本 |

---

## 環境變數

| 變數 | 必填 | 說明 | 預設值 |
| :------- | :------: | :---------- | :------ |
| `MCP_API_KEY` | 否* | 你的 Muggle Test API 金鑰 | - |
| `PROMPT_SERVICE_BASE_URL` | 否 | 覆寫 API 端點 | 正式環境網址 |
| `ELECTRON_APP_PATH` | 否 | 自訂瀏覽器引擎路徑 | 自動下載 |
| `LOG_LEVEL` | 否 | 記錄詳細程度 | `info` |

*使用雲端功能時需要 API 金鑰。本機測試不需身分驗證即可運作。

---

## 身分驗證

### 選項 A：代理程式輔助身分驗證（建議）

讓你的 AI 助理協助你完成身分驗證：

1. 設定 MCP 伺服器（不含 API 金鑰）
2. 提問：**「幫我登入 Muggle Test」**
3. 助理會呼叫 `muggle-remote-auth-login`
4. 前往提供的網址並完成登入
5. 你的憑證會儲存在本機

### 選項 B：手動設定 API 金鑰

1. 登入 [Muggle Test 儀表板](https://www.muggle-ai.com)
2. 前往 **設定** → **API 金鑰**
3. 建立並複製你的 API 金鑰
4. 將其加入你的 MCP 設定

---

## 驗證安裝

設定好 MCP 用戶端後：

| 平台 | 驗證方式 |
| :------- | :------------ |
| **Claude Code** | 執行 `/muggle:status` |
| **Cursor** | 重新啟動 Cursor，然後提問：**「檢查 Muggle Test 狀態」** |
| **Antigravity** | 重新啟動 Antigravity，然後提問：**「檢查 Muggle Test 狀態」** |
| **Claude Desktop** | 重新啟動應用程式，然後提問：**「檢查 Muggle Test 狀態」** |

如果出現錯誤，請執行 `muggle doctor` 進行診斷。

---

## 架構

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Your Computer                                │
│                                                                     │
│  ┌────────────────┐          ┌─────────────────────────────────┐   │
│  │ Cursor or      │  spawns  │ @muggleai/works                   │   │
│  │ Claude Desktop │─────────►│ (runs as child process)         │   │
│  │                │          │                                 │   │
│  │                │  stdin   │ Local Tools → Browser Engine    │   │
│  │ "Test my app"  │─────────►│ Cloud Tools → Muggle API        │   │
│  │                │          │                                 │   │
│  │                │  stdout  │ Returns results                 │   │
│  │ ◄──────────────│──────────│                                 │   │
│  └────────────────┘          └─────────────────────────────────┘   │
│                                          │                         │
└──────────────────────────────────────────│─────────────────────────┘
                                           │ HTTPS (cloud tools only)
                                           ▼
                              ┌─────────────────────────┐
                              │ Muggle Test Platform    │
                              │ (promptservice.muggle-ai.com)│
                              └─────────────────────────┘
```

---

## 資料儲存

執行結果、偏好設定與憑證會儲存在 `~/.muggle-ai/` 中。專案、使用者旅程與測試案例則存放在你的 Muggle Test 雲端帳戶中，而不是在磁碟上。

```
~/.muggle-ai/
├── credentials.json           # API credentials
├── auth.json                  # Authentication tokens
├── preferences.json           # Your testing preferences
├── electron-app/              # Browser automation engine
│   └── {version}/
└── sessions/                  # Local run results and screenshots
```

---

## 疑難排解

### 工具未顯示

| 檢查項目 | 解決方法 |
| :---- | :------- |
| 是否已安裝套件？ | 執行 `npm list -g @muggleai/works` |
| 是否已重新啟動用戶端？ | 變更設定後，請重新啟動你的 MCP 用戶端 |
| Node 版本？ | 確認使用 Node.js 22+（`node --version`） |
| 設定是否正確？ | 檢查 MCP 設定中的 JSON 語法 |

### 平台設定路徑

| 平台 | 設定檔位置 |
| :------- | :------------------- |
| Claude Code | 透過外掛程式系統管理——不需要手動設定 |
| Cursor | `~/.cursor/mcp.json` |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` |
| Claude Desktop（macOS） | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop（Windows） | `%APPDATA%\Claude\claude_desktop_config.json` |

### "Electron-app not found"

| 檢查項目 | 解決方法 |
| :---- | :------- |
| 下載失敗？ | 執行 `muggle setup` 重試 |
| 網路問題？ | 檢查網際網路連線與代理伺服器設定 |
| 自訂位置？ | 設定 `ELECTRON_APP_PATH` 環境變數 |

### 身分驗證錯誤

| 錯誤 | 解決方法 |
| :---- | :------- |
| "Device code expired" | 裝置碼的有效期限為 15 分鐘。請重新開始。 |
| "Not authenticated" | 執行 `muggle login` 或設定 API 金鑰 |
| 權杖已過期 | 使用 `muggle login` 重新進行身分驗證 |

如需更多協助，請參閱[疑難排解](troubleshooting/common-issues.md)。

---

## 後續步驟

- **[MCP 快速入門](getting-started/mcp-quickstart.md)**——執行你的第一個測試
- **[本機測試安裝與設定](local-testing/setup.md)**——測試 localhost 應用程式
- **[MCP 概念與架構](mcp-concepts.md)**——了解架構
- **[MCP API 參考](mcp-api-reference.md)**——完整的工具文件
