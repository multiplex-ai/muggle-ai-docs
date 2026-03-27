# Plugin Skills

Pre-built AI agent skills that automate testing and development workflows with Muggle AI.

## What are Plugin Skills?

Plugin skills are structured workflows that teach AI assistants how to perform complex, multi-step tasks. Instead of manually calling individual tools, you invoke a skill and the agent follows the workflow automatically.

## Installation

Skills are delivered through the Muggle AI plugin for Claude Code:

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

After installation, skills are available as `/muggle:*` commands.

## Available Skills

| Skill | Description | Example trigger |
| :---- | :---------- | :-------------- |
| `/muggle:do` | Autonomous dev pipeline: requirements to PR | "Add a logout button to the header" |
| `/muggle:test-feature-local` | Test features on localhost with browser QA | "Test my login flow on localhost:3000" |
| `/muggle:status` | Health check for QA engine, MCP server, auth | "Check muggle status" |
| `/muggle:repair` | Diagnose and fix broken installation | "Repair my muggle setup" |
| `/muggle:upgrade` | Update to the latest version | "Upgrade muggle" |

## /muggle:do

Full autonomous development cycle: the AI codes the feature, runs unit tests, QA tests the app in a real browser across desktop and mobile viewports, triages failures, and opens a PR with evidence.

### Example

```
/muggle:do "Add a logout button to the header"

REQUIREMENTS  -> Goal: Add logout button
IMPACT        -> frontend repo, src/components/Header.tsx
CODING        -> (writes code)
UNIT_TESTS    -> 12/12 pass
QA            -> 3/3 test cases pass
OPEN_PRS      -> PR #42 opened
```

Features:

- Session-based with crash recovery
- Auto-triage: analyzes failures and loops back to fix (max 3 iterations)
- Multi-repo support via `muggle-repos.json`
- PRs include QA results and screenshots

## /muggle:test-feature-local

Test a feature running on localhost with AI-driven browser automation.

### Example

```
/muggle:test-feature-local

"Test my login changes on localhost:3000"

1. Auth check
2. Found project: "My App"
3. Found 2 test cases - recommend replay
4. Launching QA engine... (approve? y)
5. Results: 2/2 PASS
6. Publish to cloud? (y)
```

The skill handles the full workflow: authenticate, find or create test cases, execute in a real browser, report results with screenshots, and optionally publish the generated script to cloud.

## /muggle:status

Checks the health of your Muggle AI installation:

- Electron QA engine version and binary integrity
- MCP server responsiveness
- Authentication state and token expiry

If any check fails, run `/muggle:repair`.

## /muggle:repair

Automatically diagnoses and fixes broken components:

- Re-downloads the Electron QA engine if missing or corrupt
- Re-authenticates if credentials are expired
- Reports what was repaired

## /muggle:upgrade

Updates all Muggle AI components to the latest version and reports before/after versions.

## Skills vs Direct MCP Tool Calls

| Approach | When to use |
| :------- | :---------- |
| **Skills** | Common workflows, multi-step tasks, "just build it" or "just test it" |
| **Direct tools** | Fine-grained control, specific operations, custom workflows |

Skills call the same 70+ MCP tools under the hood -- they automate the orchestration.

## Next Steps

- **[Tools Reference](tools-reference.md)** -- Individual tool documentation
- **[Example Workflows](examples.md)** -- See tools and skills in action
- **[Local Testing Setup](setup.md)** -- Installation and configuration
