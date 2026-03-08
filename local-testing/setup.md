# Local Testing Setup

Install and configure Muggle Test Local to test your localhost applications.

## Prerequisites

| Requirement | Details |
| :---------- | :------ |
| **Node.js** | Version 18 or later |
| **MCP Client** | Cursor IDE, Claude Desktop, or any MCP-compatible assistant |
| **Disk Space** | ~100MB for packages, plus space for test sessions |

## Step 1: Install the Packages

Clone and build the Muggle Test Local packages:

```bash
git clone https://github.com/anthropic/muggle-ai-teaching-service
cd muggle-ai-teaching-service
npm install
npm run build
```

This installs two key components:

| Component | Purpose |
| :-------- | :------ |
| `local-mcp` | MCP server that your AI assistant communicates with |
| `web-service` | Manages the browser automation engine |

## Step 2: Configure Your AI Assistant

Add the MCP server to your assistant's configuration.

### Cursor IDE

Create or edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "muggle-test-local": {
      "command": "node",
      "args": ["/path/to/muggle-ai-teaching-service/packages/local-mcp/dist/index.js"],
      "env": {
        "WEB_SERVICE_URL": "http://localhost:3001"
      }
    }
  }
}
```

> **Important**: Replace `/path/to/` with the actual absolute path where you cloned the repository.

### Claude Desktop

Edit your Claude Desktop configuration:

| Platform | Config File Location |
| :------- | :------------------- |
| macOS | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

```json
{
  "mcpServers": {
    "muggle-test-local": {
      "command": "node",
      "args": ["/path/to/muggle-ai-teaching-service/packages/local-mcp/dist/index.js"],
      "env": {
        "WEB_SERVICE_URL": "http://localhost:3001"
      }
    }
  }
}
```

## Step 3: Start the Web Service

The web service manages the browser automation engine. Start it in a terminal:

```bash
cd muggle-ai-teaching-service/packages/web-service
STORAGE_MODE=local npm start
```

You should see output indicating the server is running:

```
Web service started on port 3001
Storage mode: local
```

> **Tip**: Keep this terminal open while testing. The web service must be running for local testing to work.

## Step 4: Restart Your AI Assistant

Restart Cursor or Claude Desktop to load the new MCP server configuration.

## Step 5: Verify the Connection

Ask your AI assistant to check the connection:

> "Check the status of Muggle Test Local"

You should see a response confirming the connection is healthy:

```
Muggle Test Local Status:
- Connection: ✓ Healthy
- Web Service: Running at http://localhost:3001
- Storage: ~/.muggle-ai/sessions/
```

## Running Your First Test

Once connected, try a simple test:

> "Explore the page at http://localhost:3000"

The assistant will:
1. Launch a browser
2. Navigate to your local app
3. Analyze the page
4. Return a summary of interactive elements

## Configuration Options

### Environment Variables

You can customize behavior with environment variables in your MCP config:

| Variable | Default | Description |
| :------- | :------ | :---------- |
| `WEB_SERVICE_URL` | `http://localhost:3001` | URL of the web service |
| `LOG_LEVEL` | `info` | Logging verbosity (`debug`, `info`, `warn`, `error`) |

### Optional: Authentication

For usage tracking and cloud features, you can optionally authenticate:

```json
{
  "env": {
    "WEB_SERVICE_URL": "http://localhost:3001",
    "AUTH0_DOMAIN": "your-tenant.auth0.com",
    "AUTH0_CLIENT_ID": "your-client-id",
    "AUTH0_CLIENT_SECRET": "your-client-secret",
    "AUTH0_AUDIENCE": "your-api-audience"
  }
}
```

> **Note**: Authentication is optional for local testing. It's only needed if you want to sync with cloud features.

## Data Storage

All test data is stored locally in `~/.muggle-ai/`:

```
~/.muggle-ai/
├── auth.json                    # Authentication tokens (if configured)
├── logs/                        # Server logs
└── sessions/
    ├── current -> session-xxx/  # Symlink to active session
    └── session-xxx/
        ├── metadata.json        # Session information
        ├── results.md           # Test results in markdown
        └── screenshots/         # Captured screenshots
```

## Troubleshooting

### "Cannot connect to web-service"

| Check | Solution |
| :---- | :------- |
| Web service running? | Start it with `STORAGE_MODE=local npm start` |
| Correct port? | Verify `WEB_SERVICE_URL` matches the web service port |
| Firewall? | Ensure localhost connections aren't blocked |

### Tools Not Appearing

| Check | Solution |
| :---- | :------- |
| Path correct? | Verify the absolute path in your MCP config |
| Package built? | Run `npm run build` in `packages/local-mcp` |
| Client restarted? | Restart Cursor/Claude Desktop after config changes |

### "No active browser session"

This means no browser is currently open. Start a test first:

> "Navigate to http://localhost:3000"

For more troubleshooting help, see [Common Issues](troubleshooting/common-issues.md).

## Next Steps

- **[Available Tools](local-testing/tools-reference.md)** — Learn what tools are available
- **[Example Workflows](local-testing/examples.md)** — Common testing patterns
- **[Local Testing Overview](local-testing/overview.md)** — Understanding the architecture
