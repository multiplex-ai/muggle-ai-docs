# Local Testing Setup

Install and configure Muggle Test to test your localhost applications.

## Prerequisites

| Requirement | Details |
| :---------- | :------ |
| **Node.js** | Version 22 or later |
| **MCP Client** | Cursor IDE, Claude Desktop, or any MCP-compatible assistant |
| **Disk Space** | ~500MB for packages (includes bundled browser engine) |

## Step 1: Install

**Claude Code (recommended):**

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

**npm (for Cursor or CLI-only use):**

```bash
npm install -g @muggleai/works@latest
```

During installation, the package automatically downloads the platform-specific browser engine.

The package includes:

| Component | Purpose |
| :-------- | :------ |
| `muggle` | Unified MCP server and CLI |
| Browser Engine | Electron-based browser automation (downloaded per-platform) |
| Local Tools | 60+ tools for local testing |
| Cloud Tools | 60+ tools for cloud QA (optional) |

### Verify Installation

After installation, verify everything is set up correctly:

```bash
muggle doctor
```

This checks:
- Node.js version
- Browser engine installation
- Data directory status
- Authentication status

**Expected output:**

```
Muggle Doctor
=============
✓ Data Directory: Found at ~/.muggle-ai
✓ Electron App: Installed (v1.0.0)
✗ Authentication: Not authenticated
  └─ Run 'muggle login' to authenticate
✓ Prompt Service URL: https://promptservice.muggle-ai.com
```

## Step 2: Configure Your AI Assistant

Add the MCP server to your assistant's configuration.

### Cursor IDE

Edit `~/.cursor/mcp.json`:

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

**For local testing only** (no cloud features):

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

Edit your Claude Desktop configuration:

| Platform | Config File Location |
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

## Step 3: Restart Your AI Assistant

Restart Cursor or Claude Desktop to load the new MCP server configuration.

## Step 4: Verify the Connection

Ask your AI assistant to check the connection:

> "Check the Muggle Test status"

You should see a response indicating the MCP is ready:

```
Muggle Test Local Status
======================
Data Directory: ~/.muggle-ai
Sessions Directory: ~/.muggle-ai/sessions

Authentication
--------------
Authenticated: No
```

## Running Your First Test

Once connected, try creating a project:

> "Create a test project for my app at http://localhost:3000"

The assistant will:
1. Create a local project in `~/.muggle-ai/projects/`
2. Return the project details
3. Be ready for you to add use cases and test cases

## CLI Commands

The `muggle` command provides helpful utilities:

| Command | Description |
| :------ | :---------- |
| `muggle serve` | Start the MCP server (default: all tools) |
| `muggle serve --local` | Start with local tools only |
| `muggle serve --qa` | Start with cloud QA tools only |
| `muggle setup` | Download/install the browser engine |
| `muggle setup --force` | Force re-download even if installed |
| `muggle doctor` | Check installation and diagnose issues |
| `muggle login` | Authenticate with Muggle AI |
| `muggle logout` | Clear stored credentials |
| `muggle status` | Show authentication status |

## Configuration Options

### Environment Variables

You can customize behavior with environment variables in your MCP config:

| Variable | Default | Description |
| :------- | :------ | :---------- |
| `LOG_LEVEL` | `info` | Logging verbosity (`debug`, `info`, `warn`, `error`) |
| `ELECTRON_APP_PATH` | (auto-downloaded) | Custom path to browser engine executable |
| `MCP_API_KEY` | - | API key for cloud features |
| `PROMPT_SERVICE_BASE_URL` | (production) | Override the prompt service URL |

**Example with custom settings:**

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

## Authentication (Optional)

Authentication is **optional for local testing**. It's only needed if you want to:
- Publish local projects to the cloud
- Pull cloud projects to test locally
- Use cloud QA features

### Device Code Flow

1. Ask your assistant: "Log in to Muggle Test"
2. You'll receive a URL and code to open in your browser
3. Complete login in your browser
4. The assistant polls for completion automatically

**Example flow:**

> **You**: "Log in to Muggle Test"
>
> **Assistant**: Please complete authentication:
> - Open: `https://auth.muggle-ai.com/activate?user_code=ABCD-1234`
> - After logging in, I'll confirm your authentication.
>
> **You**: "Done, check if I'm logged in"
>
> **Assistant**: Authentication successful! Logged in as user@example.com

Authentication tokens are stored locally at `~/.muggle-ai/auth.json`.

## Data Storage

All test data is stored locally in `~/.muggle-ai/`:

```
~/.muggle-ai/
├── auth.json                    # Authentication tokens
├── credentials.json             # API credentials
├── electron-app/                # Browser automation engine
│   └── {version}/
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
| Package installed? | Run `npm list -g @muggleai/works` |
| Config correct? | Verify JSON syntax in your MCP config file |
| Client restarted? | Restart Cursor/Claude Desktop after config changes |
| Node version? | Ensure Node.js 22+ is installed (`node --version`) |

### "Electron-app not found"

The browser engine is downloaded automatically during installation. If you see this error:

| Check | Solution |
| :---- | :------- |
| Download failed? | Run `muggle setup` to retry download |
| Network issues? | Check your internet connection and proxy settings |
| Custom location? | Set `ELECTRON_APP_PATH` environment variable |
| Platform supported? | macOS (arm64, x64), Windows (x64), and Linux (x64) are supported |

### "Download failed with status 404"

This means the release asset wasn't found. Common causes:

| Cause | Solution |
| :---- | :------- |
| New version not published | Check [available releases](https://github.com/multiplex-ai/muggle-ai-works/releases) |
| Development version | For local development, build manually (see below) |

**Building from source (for developers):**

```bash
git clone https://github.com/multiplex-ai/muggle-ai-works
cd muggle-ai-works
npm install
npm run build
```

Or set the `ELECTRON_APP_PATH` environment variable in your MCP config.

### Authentication Errors

| Error | Solution |
| :---- | :------- |
| "Device code expired" | Codes are valid for 15 minutes. Start login again. |
| "Authorization pending" | Complete the browser login, then check status. |
| Token expired | Run `muggle login` to get a new token. |

For more troubleshooting help, see [Common Issues](../troubleshooting/common-issues.md).

## Next Steps

- **[Local Testing Overview](overview.md)** — Understanding the architecture
- **[Available Tools](tools-reference.md)** — Learn what tools are available
- **[Example Workflows](examples.md)** — Common testing patterns
- **[Agent Skills](skills.md)** — Pre-built testing workflows
