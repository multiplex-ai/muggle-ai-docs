# MCP Gateway Installation

The MCP QA Gateway can be used in two ways: **Hosted** (no installation) or **Self-Hosted** (local installation).

## Deployment Options

| Option | Best For | Setup |
| :----- | :------- | :---- |
| **Hosted** | Quick start, no maintenance | Just configure your MCP client |
| **Self-Hosted** | Lower latency, privacy, offline use | Install npm package locally |

---

## Option 1: Hosted Gateway (Recommended)

Use Muggle AI's hosted MCP gateway. No installation required.

### Configuration

**Endpoint:** `https://mcp.muggle-ai.com/mcp`

**Transport:** Streamable HTTP

**Authentication:** API Key via `x-api-key` header

See [MCP Quickstart](../getting-started/mcp-quickstart.md) for client configuration.

---

## Option 2: Self-Hosted (Local Installation)

Run the MCP gateway locally on your machine. The gateway runs as a subprocess of your AI assistant, communicating via stdin/stdout.

### Benefits

| Benefit | Description |
| :------ | :---------- |
| **Lower Latency** | No network round-trip to hosted gateway |
| **Privacy** | Credentials stay on your machine |
| **Offline Development** | Works without internet (requires prompt-service access) |
| **Customization** | Modify for custom needs |

### Prerequisites

| Requirement | Version |
| :---------- | :------ |
| Node.js | 18+ |
| npm | 8+ |

### Installation

```bash
npm install -g @muggleai/mcp-qa-gateway
```

Or run directly with npx (no installation):

```bash
npx @muggleai/mcp-qa-gateway --stdio
```

### How It Works

When you configure your MCP client to use the local gateway:

```
┌─────────────────────────────────────────────────────────────────────┐
│                        Your Computer                                │
│                                                                     │
│  ┌────────────────┐          ┌─────────────────────────────────┐   │
│  │ Claude Desktop │  spawns  │ @muggleai/mcp-qa-gateway        │   │
│  │ or Cursor      │─────────►│ (runs as child process)         │   │
│  │                │          │                                 │   │
│  │                │  stdin   │ Receives: {"method":"tools/call"}│   │
│  │ "Run QA tests" │─────────►│                                 │   │
│  │                │          │                                 │   │
│  │                │  stdout  │ Returns: {"result": {...}}      │   │
│  │ ◄──────────────│──────────│                                 │   │
│  └────────────────┘          └─────────────────────────────────┘   │
│                                          │                         │
└──────────────────────────────────────────│─────────────────────────┘
                                           │ HTTPS
                                           ▼
                              ┌─────────────────────────┐
                              │ Muggle Test Platform    │
                              │ (promptservice.muggle-ai.com)│
                              └─────────────────────────┘
```

1. Your AI assistant starts the gateway as a subprocess
2. Commands are sent via stdin (JSON-RPC format)
3. The gateway calls Muggle Test APIs
4. Results return via stdout
5. Logs go to stderr (don't interfere with protocol)

---

## Client Configuration

### Claude Desktop (Local Gateway)

Edit your Claude Desktop configuration file:

| Platform | Path |
| :------- | :--- |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "muggle-test": {
      "command": "npx",
      "args": ["@muggleai/mcp-qa-gateway", "--stdio"],
      "env": {
        "PROMPT_SERVICE_BASE_URL": "https://promptservice.muggle-ai.com",
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

### Claude Desktop (Hosted Gateway)

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

### Cursor IDE (Local Gateway)

Add to Cursor MCP settings (Settings → MCP):

```json
{
  "mcp": {
    "servers": {
      "muggle-test": {
        "command": "npx",
        "args": ["@muggleai/mcp-qa-gateway", "--stdio"],
        "env": {
          "PROMPT_SERVICE_BASE_URL": "https://promptservice.muggle-ai.com",
          "MCP_API_KEY": "mai_sk_your_api_key_here"
        }
      }
    }
  }
}
```

### Cursor IDE (Hosted Gateway)

```json
{
  "mcp": {
    "servers": {
      "muggle-test": {
        "url": "https://mcp.muggle-ai.com/mcp",
        "headers": {
          "x-api-key": "mai_sk_your_api_key_here"
        }
      }
    }
  }
}
```

---

## Environment Variables

| Variable | Required | Description | Default |
| :------- | :------: | :---------- | :------ |
| `PROMPT_SERVICE_BASE_URL` | Yes | Muggle Test API endpoint | - |
| `MCP_API_KEY` | Yes* | Your Muggle Test API key | - |
| `MCP_BEARER_TOKEN` | Yes* | Alternative: JWT token | - |
| `LOG_LEVEL` | No | Logging verbosity | `info` |
| `NODE_ENV` | No | Environment mode | `production` |

*One of `MCP_API_KEY` or `MCP_BEARER_TOKEN` is required.

---

## Verifying Installation

After configuring your MCP client, verify the connection:

1. **Restart your MCP client** (Claude Desktop, Cursor)
2. **Ask the assistant:** "List the available Muggle Test tools"
3. **Expected response:** A list of 50+ QA testing tools

If you see errors, check [Troubleshooting](../troubleshooting/common-issues.md).

---

## Comparison: Local vs Hosted

| Aspect | Local (Stdio) | Hosted (HTTP) |
| :----- | :------------ | :------------ |
| **Latency** | Lowest (no network to gateway) | Network round-trip |
| **Setup** | Install npm package | None |
| **Credentials** | Stored in config file | Sent per-request |
| **Updates** | Manual (`npm update`) | Automatic |
| **Offline** | Works (if backend accessible) | Requires internet |
| **Rate Limits** | Applied at backend | Applied at gateway + backend |

---

## Next Steps

- **[MCP Quickstart](../getting-started/mcp-quickstart.md)** - Run your first test
- **[MCP Concepts](mcp-concepts.md)** - Understand the architecture
- **[MCP API Reference](mcp-api-reference.md)** - Complete tool documentation
