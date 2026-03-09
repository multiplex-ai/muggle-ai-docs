# Local Testing Setup

Install and configure Muggle Test Local to test your localhost applications.

## Prerequisites

| Requirement | Details |
| :---------- | :------ |
| **Node.js** | Version 22 or later |
| **MCP Client** | Cursor IDE, Claude Desktop, or any MCP-compatible assistant |
| **Disk Space** | ~500MB for packages (includes bundled browser engine) |

## Step 1: Install the Package

Install the Muggle Test Local MCP package:

```bash
npm install -g @muggle-ai/local-mcp
```

Or clone and build from source:

```bash
git clone https://github.com/muggle-ai/muggle-ai-teaching-service
cd muggle-ai-teaching-service
npm install
npm run build
```

The package includes:

| Component | Purpose |
| :-------- | :------ |
| `local-mcp` | MCP server that your AI assistant communicates with |
| `electron-app` | Bundled browser automation engine (per-platform) |

## Step 2: Configure Your AI Assistant

Add the MCP server to your assistant's configuration.

### Cursor IDE

Create or edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "muggle-test-local": {
      "command": "node",
      "args": ["/path/to/muggle-ai-teaching-service/packages/local-mcp/dist/index.js"]
    }
  }
}
```

Or if installed globally:

```json
{
  "mcpServers": {
    "muggle-test-local": {
      "command": "muggle-test-local"
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
      "args": ["/path/to/muggle-ai-teaching-service/packages/local-mcp/dist/index.js"]
    }
  }
}
```

## Step 3: Restart Your AI Assistant

Restart Cursor or Claude Desktop to load the new MCP server configuration.

## Step 4: Verify the Connection

Ask your AI assistant to check the connection:

> "Check the authentication status"

You should see a response indicating the MCP is ready:

```
Authentication Status:
- Authenticated: No
- To log in, use muggle_auth_login
```

## Running Your First Test

Once connected, try creating a project:

> "Create a test project for my app at http://localhost:3000"

The assistant will:
1. Create a local project in `~/.muggle-ai/projects/`
2. Return the project details
3. Be ready for you to add use cases and test cases

## Configuration Options

### Environment Variables

You can customize behavior with environment variables in your MCP config:

| Variable | Default | Description |
| :------- | :------ | :---------- |
| `LOG_LEVEL` | `info` | Logging verbosity (`debug`, `info`, `warn`, `error`) |
| `ELECTRON_APP_PATH` | (bundled) | Custom path to Muggle AI browser engine |

### Authentication (Device Code Flow)

For cloud sync and publishing features, authenticate using the Device Code flow:

1. Ask your assistant: "Log in to Muggle Test"
2. You'll receive a URL and code to open in your browser
3. Complete login in your browser
4. Ask the assistant to poll for completion

**Example flow:**

> **You**: "Log in to Muggle Test"
>
> **Assistant**: Please complete authentication:
> - Open: `https://auth.muggle-ai.com/activate?user_code=ABCD-1234`
> - After logging in, I'll poll to complete authentication.
>
> **You**: "Done, check if I'm logged in"
>
> **Assistant**: Authentication successful! Logged in as user@example.com

Authentication tokens are stored locally at `~/.muggle-ai/auth.json` and are valid for ~24 hours.

> **Note**: Authentication is optional for local testing. It's only needed if you want to publish to the cloud.

## Data Storage

All test data is stored locally in `~/.muggle-ai/`:

```
~/.muggle-ai/
├── auth.json                    # Authentication tokens
└── projects/
    └── {project_id}/
        ├── project.json         # Project definition
        ├── use-cases/
        │   └── {use_case_id}.json
        ├── test-cases/
        │   └── {test_case_id}.json
        ├── test-scripts/
        │   └── {test_script_id}/
        │       ├── script.json
        │       └── screenshots/
        └── runs/
            └── {run_id}/
                ├── result.json
                └── screenshots/
```

## Troubleshooting

### Tools Not Appearing

| Check | Solution |
| :---- | :------- |
| Path correct? | Verify the absolute path in your MCP config |
| Package built? | Run `npm run build` in `packages/local-mcp` |
| Client restarted? | Restart Cursor/Claude Desktop after config changes |
| Node version? | Ensure Node.js 22+ is installed (`node --version`) |

### "Electron-app not found"

The browser engine is bundled with the package. If you see this error:

| Check | Solution |
| :---- | :------- |
| Fresh install? | Re-run `npm install` to get bundled binaries |
| Custom location? | Set `ELECTRON_APP_PATH` to your binary location |
| Platform supported? | macOS, Windows, and Linux are supported |

### Authentication Errors

| Error | Solution |
| :---- | :------- |
| "Device code expired" | Codes are valid for 15 minutes. Start login again. |
| "Authorization pending" | Complete the browser login, then poll again. |
| Token expired | Re-run `muggle_auth_login` to get a new token. |

For more troubleshooting help, see [Common Issues](../troubleshooting/common-issues.md).

## Next Steps

- **[Available Tools](local-testing/tools-reference.md)** — Learn what tools are available
- **[Example Workflows](local-testing/examples.md)** — Common testing patterns
- **[Local Testing Overview](local-testing/overview.md)** — Understanding the architecture
