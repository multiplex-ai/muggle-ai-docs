# MCP Installation

The Muggle AI MCP package provides a unified server for both **Cloud QA testing** and **Local testing** through a single installation.

## Package Overview

The `@muggleai/works` package combines:

| Feature | Description |
| :------ | :---------- |
| **Cloud QA Tools** | Test preview, staging, and production environments |
| **Local Testing Tools** | Test localhost applications with browser automation |
| **Unified CLI** | Single command-line interface for all features |
| **Shared Authentication** | One login for both local and cloud features |

---

## Quick Install

**Claude Code (recommended):**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

This delivers skills, MCP server, and hooks through the plugin lifecycle. No manual file copying.

**npm (for Cursor or CLI-only use):**

```bash
npm install -g @muggleai/works@latest
```

This installs:
- The MCP server (`muggle serve`)
- Browser automation engine (downloaded automatically)
- CLI tools for diagnostics and authentication

---

## Deployment Options

| Option | Best For | Setup |
| :----- | :------- | :---- |
| **Hosted Gateway** | Quick start, cloud-only testing | Just configure your MCP client |
| **Local Installation** | Local testing, lower latency, privacy | Install npm package |

### Option 1: Hosted Gateway (Cloud Testing Only)

Use Muggle AI's hosted MCP gateway for testing remote URLs. No installation required.

**Endpoint:** `https://mcp.muggle-ai.com/mcp`

**Transport:** Streamable HTTP

**Authentication:** API Key via `x-api-key` header

```json
{
  "mcpServers": {
    "muggle-test": {
      "url": "https://mcp.muggle-ai.com/mcp",
      "headers": {
        "x-api-key": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

### Option 2: Local Installation (Recommended)

Install locally to enable **both local testing and cloud QA** through a single MCP server.

#### Benefits

| Benefit | Description |
| :------ | :---------- |
| **Local Testing** | Test localhost and local network URLs |
| **Lower Latency** | No network round-trip to hosted gateway |
| **Privacy** | Credentials stay on your machine |
| **Unified Experience** | One server for all testing needs |

#### Prerequisites

| Requirement | Version |
| :---------- | :------ |
| Node.js | 22+ |
| npm | 10+ |

#### Installation

```bash
npm install -g @muggleai/works@latest
```

During installation, the browser automation engine is automatically downloaded for your platform.

#### Verify Installation

```bash
muggle doctor
```

This checks:
- Node.js version
- Browser engine installation
- Data directory status
- Authentication status

---

## Client Configuration

### Cursor IDE

Edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "muggle-test": {
      "command": "muggle",
      "args": ["serve"]
    }
  }
}
```

**With API key (for cloud features):**

```json
{
  "mcpServers": {
    "muggle-test": {
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

Edit your Claude Desktop configuration:

| Platform | Path |
| :------- | :--- |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "muggle-test": {
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

## Tool Selection

By default, the server enables **all tools** (both local and cloud). Use flags to limit scope:

| Command | Tools Enabled |
| :------ | :------------ |
| `muggle serve` | All tools (local + cloud) |
| `muggle serve --local` | Local testing only |
| `muggle serve --qa` | Cloud QA only |

**Example: Local-only configuration:**

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

## CLI Commands

| Command | Description |
| :------ | :---------- |
| `muggle serve` | Start the MCP server |
| `muggle setup` | Download/reinstall browser engine |
| `muggle doctor` | Check installation and diagnose issues |
| `muggle login` | Authenticate with Muggle AI |
| `muggle logout` | Clear stored credentials |
| `muggle status` | Show authentication status |
| `muggle --version` | Show package version |

---

## Environment Variables

| Variable | Required | Description | Default |
| :------- | :------: | :---------- | :------ |
| `MCP_API_KEY` | No* | Your Muggle Test API key | - |
| `PROMPT_SERVICE_BASE_URL` | No | Override API endpoint | Production URL |
| `ELECTRON_APP_PATH` | No | Custom browser engine path | Auto-downloaded |
| `LOG_LEVEL` | No | Logging verbosity | `info` |

*API key is required for cloud features. Local testing works without authentication.

---

## Authentication

### Option A: Agentic Authentication (Recommended)

Let your AI assistant help you authenticate:

1. Configure the MCP server (without API key)
2. Ask: **"Help me log in to Muggle Test"**
3. The assistant calls `muggle-remote-auth-login`
4. Visit the provided URL and complete login
5. Your credentials are stored locally

### Option B: Manual API Key

1. Log in to [Muggle Test Dashboard](https://app.muggle-ai.com)
2. Navigate to **Settings** → **API Keys**
3. Create and copy your API key
4. Add to your MCP configuration

---

## Verifying Installation

After configuring your MCP client:

1. **Restart your MCP client** (Cursor, Claude Desktop)
2. **Ask the assistant:** "Check the Muggle Test status"
3. **Expected response:** Status showing available tools and auth state

If you see errors, run `muggle doctor` to diagnose.

---

## Architecture

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

## Data Storage

All local data is stored in `~/.muggle-ai/`:

```
~/.muggle-ai/
├── credentials.json           # API credentials
├── auth.json                  # Authentication tokens
├── electron-app/              # Browser automation engine
│   └── {version}/
└── projects/                  # Local test projects
    └── {project_id}/
        ├── project.json
        ├── use-cases/
        ├── test-cases/
        └── test-scripts/
```

---

## Troubleshooting

### Tools Not Appearing

| Check | Solution |
| :---- | :------- |
| Package installed? | Run `npm list -g @muggleai/works` |
| Client restarted? | Restart Cursor/Claude Desktop after config changes |
| Node version? | Ensure Node.js 22+ (`node --version`) |
| Config correct? | Verify JSON syntax in your MCP config |

### "Electron-app not found"

| Check | Solution |
| :---- | :------- |
| Download failed? | Run `muggle setup` to retry |
| Network issues? | Check internet connection and proxy settings |
| Custom location? | Set `ELECTRON_APP_PATH` environment variable |

### Authentication Errors

| Error | Solution |
| :---- | :------- |
| "Device code expired" | Codes are valid for 15 minutes. Start again. |
| "Not authenticated" | Run `muggle login` or configure API key |
| Token expired | Re-authenticate with `muggle login` |

For more help, see [Troubleshooting](../troubleshooting/common-issues.md).

---

## Next Steps

- **[MCP Quickstart](../getting-started/mcp-quickstart.md)** - Run your first test
- **[Local Testing Setup](../local-testing/setup.md)** - Test localhost applications
- **[MCP Concepts](mcp-concepts.md)** - Understand the architecture
- **[MCP API Reference](mcp-api-reference.md)** - Complete tool documentation
