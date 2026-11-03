# Muggle Test in Windsurf and Other MCP Clients

Register the Muggle Works MCP server to let Windsurf, or any other MCP client, run real-browser E2E tests on your site. These clients get the MCP tools only, without the skills or guardrail hooks. Requires Node.js 22 or later.

## Install

```bash
npm install -g @muggleai/works
```

Then register the server in your client's MCP config:

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MUGGLE_MCP_PROMPT_SERVICE_TARGET": "production"
      }
    }
  }
}
```

Restart the client to load the server.

## Setup

```bash
muggle init
```

`muggle init` explains how the harness works and saves your preferences. Then confirm the install is healthy:

```bash
muggle doctor
```

If something is broken, run `muggle setup --force`.

## First Test

Describe the flow to your agent in plain English, naming the URL:

> "Use Muggle to test the signup flow on http://localhost:3000"

The agent calls the Muggle MCP tools to generate a test script, run it in a real browser, and return the result with screenshots. The script persists, so it re-runs as a regression test after later changes.

## Fixing What Breaks

Without the skills, the agent fixes failures from the run's results. Ask it to fix the failing step and re-run the test:

> "Fix what made the signup test fail, then run it again"

## Sign-In

The first protected call opens a browser with a verification code. Sign in, and the call continues. Credentials persist in `~/.muggle-ai/`. To sign in ahead of time, run `muggle login`.

If login fails with `unauthorized_client`, the server's `MUGGLE_MCP_PROMPT_SERVICE_TARGET` does not match the account you are signing in to. Fix it and restart the client.

## Next Steps

- **[Available Tools](local-testing/tools-reference.md)** — Every MCP tool
- **[Local Testing Setup](local-testing/setup.md)** — MCP configuration options and CLI commands
- **[Common Issues](troubleshooting/common-issues.md)** — Fixes for frequent problems
