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
| `/muggle:do` | Quality-guaranteed development workflow: requirements, coding, testing, QA with real-browser validation, and PR creation | "Add a logout button and make sure it works before opening a PR" |
| `/muggle:test` | Change-driven QA router — detects code changes, maps them to use cases, runs test generation locally or remotely, publishes results to the dashboard, opens them in a browser, and posts a QA summary to the PR | "Test my changes and post results to the PR" |
| `/muggle:test-feature-local` | Run a real-browser QA test against localhost to verify a feature works — signup flows, checkout, form validation, UI interactions, or any user-facing behavior | "Test my login flow on localhost:3000" |
| `/muggle:status` | Health check for QA engine, MCP server, auth | "Check muggle status" |
| `/muggle:repair` | Diagnose and fix broken installation | "Repair my muggle setup" |
| `/muggle:upgrade` | Update to the latest version | "Upgrade muggle" |

## /muggle:do

Quality-guaranteed development workflow that takes a task through requirements, coding, testing, QA with real-browser validation, and PR creation. The AI codes the feature, runs unit tests, QA tests the app in a real browser across desktop and mobile viewports, triages failures, and opens a PR with evidence.

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


## /muggle:test

Change-driven QA router that detects code changes, resolves impacted use cases, and executes test generation either locally (Electron browser on localhost) or remotely (Muggle cloud on a preview/staging URL). After execution, it publishes results to the Muggle AI dashboard, opens them in your browser, and posts a QA summary with test case links to your PR.

### Two modes

| Mode | When to use | Execution tool |
| :--- | :---------- | :------------- |
| **Local** | Testing against `localhost` during development | `muggle-local-execute-test-generation` |
| **Remote** | Testing against a preview or staging URL | `muggle-remote-workflow-start-test-script-generation` |

The skill asks which mode you want before proceeding. If your prompt mentions "localhost" or "local", it selects local mode. If it mentions "staging", "preview", or a non-localhost URL, it selects remote mode.

### Example

```
/muggle:test

"Test my changes and post the results to the PR"

1. Intent: local or remote? -> "local, localhost:3000"
2. Changes detected: signup form refactor, profile page update
3. Auth: authenticated
4. Project: "My App" (matched)
5. Use cases: 2 existing, 1 new -> confirm mapping
6. Test cases: 4 total -> confirm scope
7. Launching QA engine x4... (approve? y)
8. Results: 3/4 PASS, 1 FAIL
9. Published to Muggle dashboard (opened in browser)
10. QA results posted to PR #42
```

### What it does after tests run

- **Publishes** each run to the Muggle AI cloud via `muggle-local-publish-test-script`
- **Opens** the Muggle AI dashboard in your browser so you can inspect step-by-step screenshots
- **Posts** a QA results comment to the PR with a pass/fail table where each test case links to its dashboard detail page

### When to use /muggle:test vs /muggle:test-feature-local

| Scenario | Recommended skill |
| :------- | :---------------- |
| Test all use cases impacted by your changes | `/muggle:test` |
| Test on a staging/preview URL | `/muggle:test` |
| Post QA results to a PR | `/muggle:test` |
| Quick single-feature test on localhost | `/muggle:test-feature-local` |


## /muggle:test-feature-local

Run a real-browser QA test against localhost to verify a feature works correctly — signup flows, checkout, form validation, UI interactions, or any user-facing behavior. Launches a browser that executes test steps and captures screenshots.

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
