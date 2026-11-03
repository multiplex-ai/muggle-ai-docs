# Muggle Test in Cursor

Install the Muggle Works package to let Cursor run real-browser E2E tests on your site and fix what breaks. Requires Node.js 22 or later.

## Install

```bash
npm install -g @muggleai/works
```

The postinstall writes the `muggle` MCP server into `~/.cursor/mcp.json` and syncs the `muggle-*` skills into `~/.cursor/skills/`. Restart Cursor.

## Setup

```bash
muggle init
```

`muggle init` explains how the harness works, saves your preferences, and offers to install the pull-request walkthrough check. Then confirm the install is healthy:

```bash
muggle doctor
```

If something is broken, run `muggle setup --force`.

## First Test

Ask Cursor's agent to run the `muggle-test` skill:

> "Use muggle-test to test my changes"

`muggle-test` detects your changes (local diff, branch, or PR), maps them to the user flows they affect, and drives a real browser through those flows on localhost or a preview URL.

## Fixing What Breaks

Ask Cursor's agent to run the `muggle-do` skill with your request:

> "Use muggle-do to add a logout button to the header"

`muggle-do` takes a request from requirements to a pull request: it builds the change, runs the unit tests, runs browser acceptance on the affected flows, repairs failures, and opens the PR with screenshots attached.

## Sign-In

The first protected call opens a browser with a verification code. Sign in, and the call continues. Credentials persist in `~/.muggle-ai/`. To sign in ahead of time, run `muggle login`.

## Next Steps

- **[Agent Skills](local-testing/skills.md)** — Every skill and when to use it
- **[Local Testing Setup](local-testing/setup.md)** — MCP configuration options and CLI commands
- **[Common Issues](troubleshooting/common-issues.md)** — Fixes for frequent problems
