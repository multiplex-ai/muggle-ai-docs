# Muggle Test in Claude Code

Install the Muggle Works plugin to let Claude Code run real-browser E2E tests on your site and fix what breaks. The plugin ships the skills, the MCP tools, the browser runner, and the guardrail hooks.

## Install

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

## Setup

```bash
muggle init
```

`muggle init` explains how the harness works, saves your preferences, and offers to install the pull-request walkthrough check. Then confirm the install is healthy:

```
/mstatus
```

If something is broken, run `/mrepair` or `muggle setup --force`.

## First Test

```
/mtest
```

`/mtest` detects your changes (local diff, branch, or PR), maps them to the user flows they affect, and drives a real browser through those flows on localhost or a preview URL. To test one named flow against localhost, use `/mtestlocal`.

## Fixing What Breaks

```
/mdo "Add a logout button to the header"
```

`/mdo` takes a request from requirements to a pull request: it builds the change, runs the unit tests, runs browser acceptance on the affected flows, repairs failures, and opens the PR with screenshots attached.

## Sign-In

The first protected call opens a browser with a verification code. Sign in, and the call continues. Credentials persist in `~/.muggle-ai/`.

## Commands

| Command | What it does |
| :------ | :----------- |
| `/m` | Command router and menu |
| `/mtest` | Change-driven acceptance testing on your diff, PR, or branch |
| `/mtestlocal` | Test one named flow against localhost |
| `/mdo` | The full cycle: request to pull request |
| `/mpr` | Post screenshots and a pass/fail summary to a PR |
| `/mstatus` | Health check: runner, MCP server, auth |
| `/mrepair` | Diagnose and fix a broken install |
| `/mupgrade` | Update to the latest version |

Update the plugin with `/plugin update muggleai@muggle-works`.

## Next Steps

- **[Agent Skills](local-testing/skills.md)** — Every skill and when to use it
- **[How Muggle Do Works](concepts/how-muggle-do-works.md)** — The build-test-fix cycle
- **[Common Issues](troubleshooting/common-issues.md)** — Fixes for frequent problems
