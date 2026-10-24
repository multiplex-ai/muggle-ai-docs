# 本地测试安装与配置

安装并配置 Muggle Test，测试你的 localhost 应用。

## 本页内容

| 章节 | 说明 |
| :------ | :---------- |
| [前提条件](#prerequisites) | 开始之前需要准备的内容 |
| [第 1 步：安装](#step-1-install) | 安装包 |
| [第 2 步：配置你的 AI 助手](#step-2-configure-your-ai-assistant) | 各平台的 MCP 配置（Claude Code、Cursor、Antigravity） |
| [第 3 步：重启你的 AI 助手](#step-3-restart-your-ai-assistant) | 加载新配置 |
| [第 4 步：验证连接](#step-4-verify-the-connection) | 确认连接正常 |
| [运行你的第一个测试](#running-your-first-test) | 创建项目并测试 |
| [CLI 命令](#cli-commands) | 可用的命令行工具 |
| [配置选项](#configuration-options) | 环境变量和自定义设置 |
| [身份验证](#authentication) | 登录以运行测试 |
| [数据存储](#data-storage) | 本地数据的存储位置 |
| [故障排除](#troubleshooting) | 常见问题和解决方法 |
| [后续步骤](#next-steps) | 延伸阅读 |

## 前提条件

| 要求 | 详情 |
| :---------- | :------ |
| **Node.js** | 22 或更高版本 |
| **MCP 客户端** | Claude Code、Cursor、Antigravity 或任何兼容 MCP 的助手 |
| **磁盘空间** | 包约占 500MB（含内置的浏览器引擎） |

## 第 1 步：安装

**Claude Code（推荐）：**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

**npm（适用于 Cursor 或仅使用 CLI 的场景）：**

```bash
npm install -g @muggleai/works@latest
```

安装过程中，该包会自动：
- 下载对应平台的浏览器引擎
- 配置 Cursor MCP（`~/.cursor/mcp.json`）
- 将技能同步到 `~/.cursor/skills/`

该包包含：

| 组件 | 用途 |
| :-------- | :------ |
| `muggle` | 统一的 MCP 服务器和 CLI |
| 浏览器引擎 | 基于 Electron 的浏览器自动化（按平台下载） |
| 本地工具 | 约 20 个工具，用于本地执行、查看结果和发布 |
| 云端工具 | 约 80 个工具，用于云端 E2E 验收测试和实体管理 |

### 验证安装

安装完成后，验证一切是否已正确配置：

```bash
muggle doctor
```

该命令会检查：
- Node.js 版本
- 浏览器引擎的安装情况
- 数据目录状态
- 身份验证状态

**预期输出：**

```
Muggle Doctor
=============
✓ Data Directory: Found at ~/.muggle-ai
✓ Electron App: Installed (v1.0.0)
✗ Authentication: Not authenticated
  └─ Run 'muggle login' to authenticate
✓ Prompt Service URL: https://promptservice.muggle-ai.com
```

## 第 2 步：配置你的 AI 助手

### Cursor IDE

**无需手动配置。** npm 安装会自动将 muggle 服务器添加到 `~/.cursor/mcp.json`。安装后重启 Cursor 即可。

如需自定义（例如仅本地模式），请编辑 `~/.cursor/mcp.json`：

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

编辑 `~/.gemini/antigravity/mcp_config.json`（或打开 Agent 面板 → **MCP Servers** → **Manage MCP Servers** → **View raw config**）：

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

**仅用于本地测试**（不含云端功能）：

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

编辑 Claude Desktop 的配置：

| 平台 | 配置文件位置 |
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

## 第 3 步：重启你的 AI 助手

重启你的 MCP 客户端（Cursor、Antigravity 或 Claude Desktop），以加载新的 MCP 服务器配置。Claude Code 用户可以跳过此步骤。

## 第 4 步：验证连接

| 平台 | 验证方式 |
| :------- | :------------ |
| **Claude Code** | 运行 `/muggle:status` |
| **Cursor** | 对你的助手说：**“检查 Muggle Test 的状态”** |
| **Antigravity** | 对你的智能体说：**“检查 Muggle Test 的状态”** |
| **Claude Desktop** | 对你的助手说：**“检查 Muggle Test 的状态”** |

你应该会看到一条表明 MCP 已就绪的响应：

```
Muggle Test Local Status
======================
Data Directory: ~/.muggle-ai
Sessions Directory: ~/.muggle-ai/sessions

Authentication
--------------
Authenticated: No
```

## 运行你的第一个测试

连接成功后，试着创建一个项目：

> “为我的应用创建一个测试项目，应用地址是 http://localhost:3000”

助手会：
1. 如有需要先帮你登录，然后在你的 Muggle Test 账户中创建该项目
2. 返回项目详情
3. 准备就绪，等你添加用户旅程和测试用例

## CLI 命令

`muggle` 命令提供以下实用功能：

| 命令 | 说明 |
| :------ | :---------- |
| `muggle serve` | 启动 MCP 服务器（默认：全部工具） |
| `muggle serve --local` | 仅启用本地工具启动 |
| `muggle serve --e2e` | 仅启用云端 E2E 验收工具启动 |
| `muggle setup` | 下载并安装浏览器引擎 |
| `muggle setup --force` | 即使已安装也强制重新下载 |
| `muggle doctor` | 检查安装并诊断问题 |
| `muggle login` | 向 Muggle AI 进行身份验证 |
| `muggle logout` | 清除已存储的凭据 |
| `muggle status` | 显示身份验证状态 |

## 配置选项

### 环境变量

你可以在 MCP 配置中通过环境变量自定义行为：

| 变量 | 默认值 | 说明 |
| :------- | :------ | :---------- |
| `LOG_LEVEL` | `info` | 日志详细程度（`debug`、`info`、`warn`、`error`） |
| `ELECTRON_APP_PATH` | （自动下载） | 浏览器引擎可执行文件的自定义路径 |
| `MCP_API_KEY` | - | 用于云端功能的 API 密钥 |
| `PROMPT_SERVICE_BASE_URL` | （生产环境） | 覆盖 Prompt Service 的网址 |
| `MUGGLE_LLM_*` | - | 使用你自己的模型运行本地测试；参见[模型来源](local-testing/model-sources.md) |

**自定义设置示例：**

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

## 身份验证

本地测试采用**云端优先**模式：连接 MCP 服务器无需凭据，但**运行测试需要登录**。测试执行的是存储在你 Muggle Test 账户中的测试用例，因此服务器会在启动浏览器之前对你进行身份验证。发布结果和使用云端 E2E 功能同样需要登录。

### 设备码流程

1. 对你的助手说：“登录 Muggle Test”
2. 你会收到一个网址和一个设备码，需要在浏览器中打开
3. 在浏览器中完成登录
4. 助手会自动轮询登录是否完成

**示例流程：**

> **你**：“登录 Muggle Test”
>
> **助手**：请完成身份验证：
> - 打开：`https://login.muggle-ai.com/activate?user_code=ABCD-1234`
> - 登录后，我会确认你的身份验证状态。
>
> **你**：“好了，检查一下我是否已登录”
>
> **助手**：身份验证成功！已登录为 user@example.com

身份验证令牌存储在本地的 `~/.muggle-ai/auth.json` 中。

## 数据存储

运行结果、偏好设置和凭据存储在本地的 `~/.muggle-ai/` 中。项目、用户旅程和测试用例保存在你的 Muggle Test 云端账户中，而不是磁盘上。

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

## 故障排除

### 工具未显示

| 检查项 | 解决方法 |
| :---- | :------- |
| 是否已安装包？ | 运行 `npm list -g @muggleai/works` |
| 配置是否正确？ | 检查对应平台配置文件中的 JSON 语法 |
| 是否已重启客户端？ | 修改配置后重启 MCP 客户端 |
| Node 版本是否符合要求？ | 确保已安装 Node.js 22 或更高版本（`node --version`） |

**各平台的配置文件路径：**

| 平台 | 配置文件 |
| :------- | :---------- |
| Claude Code | 由插件系统管理 |
| Cursor | `~/.cursor/mcp.json` |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` |
| Claude Desktop（macOS） | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop（Windows） | `%APPDATA%\Claude\claude_desktop_config.json` |

### “Electron-app not found”

浏览器引擎会在安装过程中自动下载。如果你看到此错误：

| 检查项 | 解决方法 |
| :---- | :------- |
| 下载失败？ | 运行 `muggle setup` 重新下载 |
| 网络问题？ | 检查网络连接和代理设置 |
| 自定义位置？ | 设置 `ELECTRON_APP_PATH` 环境变量 |
| 平台是否受支持？ | 支持 macOS（arm64、x64）、Windows（x64）和 Linux（x64） |

### “Download failed with status 404”

这表示未找到对应的发布资源。常见原因：

| 原因 | 解决方法 |
| :---- | :------- |
| 新版本尚未发布 | 查看[可用的发布版本](https://github.com/multiplex-ai/muggle-ai-works/releases) |
| 开发版本 | 本地开发时请手动构建（见下文） |

**从源码构建（面向开发者）：**

```bash
git clone https://github.com/multiplex-ai/muggle-ai-works
cd muggle-ai-works
npm install
npm run build
```

或者在 MCP 配置中设置 `ELECTRON_APP_PATH` 环境变量。

### 身份验证错误

| 错误 | 解决方法 |
| :---- | :------- |
| “Device code expired” | 设备码的有效期为 15 分钟。请重新登录。 |
| “Authorization pending” | 先在浏览器中完成登录，再检查状态。 |
| 令牌已过期 | 运行 `muggle login` 获取新令牌。 |

如需更多故障排除帮助，请参阅[常见问题](troubleshooting/common-issues.md)。

## 后续步骤

- **[本地测试概览](overview.md)**——了解架构
- **[可用工具](tools-reference.md)**——了解有哪些工具可用
- **[示例工作流](examples.md)**——常见的测试模式
- **[智能体技能](skills.md)**——预置的测试工作流
