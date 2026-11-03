# Muggle Test in Codex

Install the Muggle Works plugin to let Codex run real-browser E2E tests on your site and fix what breaks. The plugin ships the skills, the MCP tools, and the guardrail hooks.

## Install

```
codex plugin marketplace add multiplex-ai/muggle-ai-works
codex plugin add muggle@muggle-works
```

Restart Codex and trust the plugin's hooks when it asks (or later, in `/hooks`).

Skills are invoked as `$muggle:<skill>`, either by full name (`$muggle:muggle-test`) or by alias (`$muggle:mtest`), or picked from `/skills`.

## Setup

```bash
muggle init
```

`muggle init` explains how the harness works, saves your preferences, and offers to install the pull-request walkthrough check. Then confirm the install is healthy:

```
$muggle:muggle-status
```

`muggle doctor` runs the same check from any terminal. If something is broken, run `muggle setup --force`.

## First Test

```
$muggle:muggle-test
```

`$muggle:muggle-test` detects your changes (local diff, branch, or PR), maps them to the user flows they affect, and drives a real browser through those flows on localhost or a preview URL.

## Fixing What Breaks

```
$muggle:muggle-do "Add a logout button to the header"
```

`$muggle:muggle-do` takes a request from requirements to a pull request: it builds the change, runs the unit tests, runs browser acceptance on the affected flows, repairs failures, and opens the PR with screenshots attached.

## Sign-In

The first protected call opens a browser with a verification code. Sign in, and the call continues. Credentials persist in `~/.muggle-ai/`.

## Codex Differences

| Area | Behavior |
| :--- | :------- |
| Long local runs | The plugin allows 30 minutes per MCP call, so local browser runs are not cut off at Codex's 60-second default |
| PR watching | `$muggle:muggle-pr-followup` handles what is outstanding each time you run it, rather than watching in the background |
| Sandbox | `gh`/`glab` and writes under `~/.muggle-ai/` ask for approval under the default sandbox |

To stop the sandbox prompts, allow both in `~/.codex/config.toml`:

```toml
[sandbox_workspace_write]
network_access = true
writable_roots = ["/absolute/path/to/home/.muggle-ai"]
```

Update the plugin with `codex plugin marketplace upgrade muggle-works`.

## Next Steps

- **[Agent Skills](local-testing/skills.md)** — Every skill and when to use it
- **[How Muggle Do Works](concepts/how-muggle-do-works.md)** — The build-test-fix cycle
- **[Common Issues](troubleshooting/common-issues.md)** — Fixes for frequent problems
