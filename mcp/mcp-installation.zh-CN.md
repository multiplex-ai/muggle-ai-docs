# MCP 安装

Muggle Test MCP 包只需一次安装，即可通过一个统一的服务器同时支持**云端 E2E 验收测试**和**本地 E2E 验收测试**。

## 本页内容

| 章节 | 说明 |
| :------ | :---------- |
| [包概览](#package-overview) | 包中包含的内容 |
| [快速安装](#quick-install) | 为 Claude Code、Cursor 或 Antigravity 安装 |
| [部署选项](#deployment-options) | 托管网关与本地安装对比 |
| [客户端配置](#client-configuration) | 各平台的 MCP 配置方法 |
| [工具选择](#tool-selection) | 使用参数限制工具范围 |
| [CLI 命令](#cli-commands) | 可用的命令行工具 |
| [环境变量](#environment-variables) | 自定义行为 |
| [身份验证](#authentication) | 设备码流程和 API 密钥 |
| [验证安装](#verifying-installation) | 确认一切正常 |
| [架构](#architecture) | 各组件如何协同工作 |
| [数据存储](#data-storage) | 本地数据的存放位置 |
| [故障排除](#troubleshooting) | 常见问题与解决方法 |
| [后续步骤](#next-steps) | 延伸阅读 |

## 包概览

`@muggleai/works` 包集成了以下功能：

| 功能 | 说明 |
| :------ | :---------- |
| **云端 E2E 验收工具** | 测试预览、预发布和生产环境 |
| **本地测试工具** | 通过浏览器自动化测试 localhost 应用 |
| **统一 CLI** | 一个命令行界面涵盖所有功能 |
| **共享身份验证** | 一次登录即可同时使用本地和云端功能 |

---

## 快速安装

**Claude Code（推荐）：**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

这会通过插件生命周期提供技能、MCP 服务器和钩子。无需手动复制文件。

**npm（适用于 Cursor 或仅使用 CLI 的场景）：**

```bash
npm install -g @muggleai/works@latest
```

这会自动安装并配置所有内容：
- MCP 服务器（`muggle serve`）
- 浏览器自动化引擎（自动下载）
- 用于诊断和身份验证的 CLI 工具
- **Cursor MCP 配置**（`~/.cursor/mcp.json`，自动写入）

---

## 部署选项

| 选项 | 适用场景 | 配置方式 |
| :----- | :------- | :---- |
| **托管网关** | 快速上手、仅云端测试 | 只需配置你的 MCP 客户端 |
| **本地安装** | 本地测试、更低延迟、隐私保护 | 安装 npm 包 |

### 选项 1：托管网关（仅云端测试）

使用 Muggle AI 托管的 MCP 网关测试远程网址。无需安装。

**端点：** `https://mcp.muggle-ai.com/mcp`

**传输方式：** Streamable HTTP

**身份验证：** 通过 `x-api-key` 请求头传递 API 密钥

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

### 选项 2：本地安装（推荐）

在本地安装后，即可通过一个 MCP 服务器同时进行**本地和云端 E2E 验收测试**。

#### 优势

| 优势 | 说明 |
| :------ | :---------- |
| **本地测试** | 测试 localhost 和局域网网址 |
| **更低延迟** | 无需经过托管网关的网络往返 |
| **隐私保护** | 凭据保留在你的机器上 |
| **统一体验** | 一个服务器满足所有测试需求 |

#### 前提条件

| 要求 | 版本 |
| :---------- | :------ |
| Node.js | 22+ |
| npm | 10+ |

#### 安装

```bash
npm install -g @muggleai/works@latest
```

安装过程中，会自动为你的平台下载浏览器自动化引擎。

#### 验证安装

```bash
muggle doctor
```

该命令会检查：
- Node.js 版本
- 浏览器引擎安装情况
- 数据目录状态
- 身份验证状态

---

## 客户端配置

### Claude Code

无需手动配置——通过插件系统安装：

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

### Cursor

`npm install -g @muggleai/works` 会自动将 muggle 服务器添加到 `~/.cursor/mcp.json`。**无需手动配置**——安装后重启 Cursor 即可。

验证方法：运行 `muggle doctor`，检查“Cursor MCP Config”是否显示为通过。

**可选：添加 API 密钥以使用云端功能**

编辑 `~/.cursor/mcp.json`，添加环境变量：

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

**使用 API 密钥（用于云端功能）：**

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

编辑你的 Claude Desktop 配置：

| 平台 | 路径 |
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

## 工具选择

默认情况下，服务器会启用**所有工具**（本地和云端）。使用参数可限制范围：

| 命令 | 启用的工具 |
| :------ | :------------ |
| `muggle serve` | 所有工具（本地 + 云端） |
| `muggle serve --local` | 仅本地测试 |
| `muggle serve --e2e` | 仅云端 E2E 验收工具 |

**示例：仅本地配置：**

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

## CLI 命令

| 命令 | 说明 |
| :------ | :---------- |
| `muggle serve` | 启动 MCP 服务器 |
| `muggle setup` | 下载/重新安装浏览器引擎 |
| `muggle doctor` | 检查安装并诊断问题 |
| `muggle login` | 向 Muggle AI 进行身份验证 |
| `muggle logout` | 清除已存储的凭据 |
| `muggle status` | 显示身份验证状态 |
| `muggle --version` | 显示包版本 |

---

## 环境变量

| 变量 | 必填 | 说明 | 默认值 |
| :------- | :------: | :---------- | :------ |
| `MCP_API_KEY` | 否* | 你的 Muggle Test API 密钥 | - |
| `PROMPT_SERVICE_BASE_URL` | 否 | 覆盖 API 端点 | 生产环境网址 |
| `ELECTRON_APP_PATH` | 否 | 自定义浏览器引擎路径 | 自动下载 |
| `LOG_LEVEL` | 否 | 日志详细程度 | `info` |

*使用云端功能时需要 API 密钥。本地测试无需身份验证即可运行。

---

## 身份验证

### 选项 A：智能体辅助身份验证（推荐）

让你的 AI 助手帮你完成身份验证：

1. 配置 MCP 服务器（不带 API 密钥）
2. 提问：**“帮我登录 Muggle Test”**
3. 助手会调用 `muggle-remote-auth-login`
4. 访问提供的网址并完成登录
5. 你的凭据会存储在本地

### 选项 B：手动配置 API 密钥

1. 登录 [Muggle Test 控制台](https://www.muggle-ai.com)
2. 前往 **设置** → **API 密钥**
3. 创建并复制你的 API 密钥
4. 将其添加到你的 MCP 配置中

---

## 验证安装

配置好 MCP 客户端后：

| 平台 | 验证方法 |
| :------- | :------------ |
| **Claude Code** | 运行 `/muggle:status` |
| **Cursor** | 重启 Cursor，然后提问：**“检查 Muggle Test 状态”** |
| **Antigravity** | 重启 Antigravity，然后提问：**“检查 Muggle Test 状态”** |
| **Claude Desktop** | 重启应用，然后提问：**“检查 Muggle Test 状态”** |

如果出现错误，请运行 `muggle doctor` 进行诊断。

---

## 架构

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

## 数据存储

运行结果、偏好设置和凭据存储在 `~/.muggle-ai/` 中。项目、用户旅程和测试用例保存在你的 Muggle Test 云端账户中，而不是在磁盘上。

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

## 故障排除

### 工具未显示

| 检查项 | 解决方法 |
| :---- | :------- |
| 是否已安装包？ | 运行 `npm list -g @muggleai/works` |
| 是否已重启客户端？ | 更改配置后重启你的 MCP 客户端 |
| Node 版本？ | 确保使用 Node.js 22+（`node --version`） |
| 配置是否正确？ | 检查 MCP 配置中的 JSON 语法 |

### 平台配置路径

| 平台 | 配置文件位置 |
| :------- | :------------------- |
| Claude Code | 通过插件系统管理——无需手动配置 |
| Cursor | `~/.cursor/mcp.json` |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` |
| Claude Desktop（macOS） | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Claude Desktop（Windows） | `%APPDATA%\Claude\claude_desktop_config.json` |

### "Electron-app not found"

| 检查项 | 解决方法 |
| :---- | :------- |
| 下载失败？ | 运行 `muggle setup` 重试 |
| 网络问题？ | 检查网络连接和代理设置 |
| 自定义位置？ | 设置 `ELECTRON_APP_PATH` 环境变量 |

### 身份验证错误

| 错误 | 解决方法 |
| :---- | :------- |
| "Device code expired" | 设备码的有效期为 15 分钟。请重新开始。 |
| "Not authenticated" | 运行 `muggle login` 或配置 API 密钥 |
| 令牌已过期 | 使用 `muggle login` 重新进行身份验证 |

更多帮助请参阅[故障排除](troubleshooting/common-issues.md)。

---

## 后续步骤

- **[MCP 快速入门](getting-started/mcp-quickstart.md)**——运行你的第一个测试
- **[本地测试安装与配置](local-testing/setup.md)**——测试 localhost 应用
- **[MCP 概念与架构](mcp-concepts.md)**——了解架构
- **[MCP API 参考](mcp-api-reference.md)**——完整的工具文档
