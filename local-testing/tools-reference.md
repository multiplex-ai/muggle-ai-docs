# Local Testing Tools Reference

Complete reference for local testing tools in the `@muggleai/works` package.

> **Architecture Note:** Local testing uses a **cloud-first** approach:
> - **`muggle-remote-*` tools**: Authentication and all entity management (projects, use cases, test cases, secrets)
> - **`muggle-local-*` tools**: Local execution, results viewing, and publishing only

## Tool Namespaces

| Namespace | Purpose | Examples |
| :-------- | :------ | :------- |
| `muggle-remote-auth-*` | Authentication | `muggle-remote-auth-login`, `muggle-remote-auth-status` |
| `muggle-remote-*` | Cloud entity management | `muggle-remote-project-create`, `muggle-remote-test-case-get` |
| `muggle-local-*` | Local execution & results | `muggle-local-execute-test-generation` |

## Local Tool Categories

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Status** | `muggle-local-check-status`, `muggle-local-list-sessions` | Check local service status |
| **Execution** | `muggle-local-execute-test-generation`, `muggle-local-execute-replay` | Run test generation and replay |
| **Results** | `muggle-local-run-result-list`, `muggle-local-run-result-get` | View execution results |
| **Scripts** | `muggle-local-test-script-list`, `muggle-local-test-script-get` | View locally generated scripts |
| **Publishing** | `muggle-local-publish-test-script` | Upload generated scripts to cloud |
| **Control** | `muggle-local-cancel-execution` | Cancel running executions |

---

## Authentication Tools (muggle-remote-auth-*)

Authentication tools are in the `muggle-remote-*` namespace because they interact with the cloud.

### muggle-remote-auth-status

Check current authentication status.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `authenticated` - Whether logged in
- `email` - User email (if authenticated)
- `expiresAt` - Token expiration time

---

### muggle-remote-auth-login

Start Device Code authentication flow.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `waitForCompletion` | boolean | No | Wait for browser login (default: true) |
| `timeoutMs` | number | No | Timeout in milliseconds (default: 120000) |

**Returns:**
- `verificationUri` - URL to open in browser
- `userCode` - Code to enter
- `deviceCode` - Code for polling (use with `muggle-remote-auth-poll`)

**Example prompt:**
> "Log in to Muggle Test"

---

### muggle-remote-auth-poll

Poll for Device Code authentication completion.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `deviceCode` | string | No | Device code from `muggle-remote-auth-login` |

**Returns:**
- `status` - `complete`, `pending`, `expired`, or `error`
- `email` - User email (if complete)

---

### muggle-remote-auth-logout

Clear stored credentials.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

---

## Status Tools

### muggle-local-check-status

Check the status of Muggle Test local service.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `dataDirectory` - Local data storage path
- `sessionsDirectory` - Sessions storage path
- `authenticated` - Whether authenticated

---

### muggle-local-list-sessions

List all stored testing sessions.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `limit` | number | No | Maximum sessions to return (default: 10) |

---

## Execution Tools

### muggle-local-execute-test-generation

Generate a QA test script by launching a real browser against your web app. The browser navigates your app, executes the test case steps (like signing up, filling forms, clicking through flows), and produces a replayable test script with screenshots. Use this to create new browser tests for any user flow.

**Important:** First call `muggle-remote-test-case-get` to fetch test case details, then pass them here.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testCase` | object | Yes | Test case details from `muggle-remote-test-case-get` |
| `localUrl` | string | Yes | Local URL to test against (e.g., `http://localhost:3000`) |
| `approveElectronAppLaunch` | boolean | Yes | Must be `true` to proceed |
| `timeoutMs` | number | No | Timeout in milliseconds (default: 300000) |

**testCase object:**
- `id` - Cloud test case ID
- `title` - Test case title
- `goal` - What the test should verify
- `expectedResult` - Expected outcome
- `precondition` - Setup requirements (optional)
- `instructions` - Step-by-step instructions (optional)

**What happens:**
1. Browser engine launches
2. Navigates to the `localUrl`
3. AI explores and records actions based on test case
4. Script is saved locally

**Example workflow:**
```
1. muggle-remote-test-case-get(testCaseId) → returns test case object
2. muggle-local-execute-test-generation({ testCase: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-execute-replay

Replay an existing QA test script in a real browser to verify your app still works correctly — use this for regression testing after code changes. The browser executes each saved step and captures screenshots so you can see what happened.

**Important:** First call `muggle-remote-test-script-get` to fetch script details, then pass them here.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testScript` | object | Yes | Test script details from `muggle-remote-test-script-get` |
| `localUrl` | string | Yes | Local URL to test against |
| `approveElectronAppLaunch` | boolean | Yes | Must be `true` to proceed |
| `timeoutMs` | number | No | Timeout in milliseconds (default: 180000) |

**testScript object:**
- `id` - Cloud test script ID
- `name` - Script name
- `testCaseId` - Parent test case ID
- `actionScript` - Array of executable steps

**URL Rewriting:**
- Original cloud URLs in the action script are automatically replaced with `localUrl`

**Example workflow:**
```
1. muggle-remote-test-script-get(testScriptId) → returns test script object
2. muggle-local-execute-replay({ testScript: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle-local-cancel-execution

Cancel a running execution.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run ID to cancel |

---

## Run Result Tools

### muggle-local-run-result-list

List execution results.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | No | Filter by cloud test case ID |
| `limit` | number | No | Maximum results (default: 20) |

---

### muggle-local-run-result-get

Get execution result details including screenshots.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run result ID |

**Returns:**
- `id` - Run ID
- `runType` - `generation` or `replay`
- `status` - Pass/fail result
- `executionTimeMs` - Duration
- `testScriptId` - Associated script (if generated)
- `errorMessage` - Error details (if failed)

---

## Test Script Tools

### muggle-local-test-script-list

List locally generated test scripts.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | No | Filter by cloud test case ID |

---

### muggle-local-test-script-get

Get test script details including action script steps.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testScriptId` | string | Yes | Local test script ID |

---

## Publishing Tools

### muggle-local-publish-test-script

Upload a locally generated test script to the cloud.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run result ID from test generation |
| `cloudTestCaseId` | string | Yes | Cloud test case ID to publish under |

**Requires:** Authentication via `muggle-remote-auth-login`

---

## Workflow: Local Testing with Cloud Entities

The recommended workflow uses cloud tools (`muggle-remote-*`) to manage entities and local tools (`muggle-local-*`) to execute:

```
┌─────────────────────────────────────────────────────────────┐
│                     Entity Management                        │
│                  (muggle-remote-* tools)                     │
├─────────────────────────────────────────────────────────────┤
│  muggle-remote-auth-login       → Authenticate               │
│  muggle-remote-project-list     → Find/create project        │
│  muggle-remote-use-case-list    → Find/create use case       │
│  muggle-remote-test-case-list   → Find/create test case      │
│  muggle-remote-test-case-get    → Fetch test case details    │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     Local Execution                          │
│                 (muggle-local-* tools)                       │
├─────────────────────────────────────────────────────────────┤
│  muggle-local-execute-test-generation → Generate locally     │
│  muggle-local-run-result-get          → View results         │
│  muggle-local-publish-test-script     → Publish to cloud     │
└─────────────────────────────────────────────────────────────┘
```

### Example: Test Login Feature on localhost:3000

```
1. muggle-remote-auth-status                         # Check auth
2. muggle-remote-auth-login                          # Login if needed
3. muggle-remote-project-list                        # Find project
4. muggle-remote-use-case-list(projectId)            # Find login use case
5. muggle-remote-test-case-list-by-use-case(ucId)    # Find login test case
6. muggle-remote-test-case-get(testCaseId)           # Get full details
7. [Ask user for approval]
8. muggle-local-execute-test-generation({
     testCase: { ...from step 6... },
     localUrl: "http://localhost:3000",
     approveElectronAppLaunch: true
   })
9. muggle-local-run-result-get(runId)           # View results
10. muggle-local-publish-test-script(runId, testCaseId)  # Publish
```

---

## Tool Selection Guide

### Test Execution

| You want to... | Use this flow |
| :------------- | :------------ |
| Generate a test script | `muggle-remote-test-case-get` → `muggle-local-execute-test-generation` |
| Replay an existing script | `muggle-remote-test-script-get` → `muggle-local-execute-replay` |
| Stop a running test | `muggle-local-cancel-execution` |
| View test results | `muggle-local-run-result-get` |

### Entity Management (use muggle-remote-* tools)

| You want to... | Use this tool |
| :------------- | :------------ |
| Create/list projects | `muggle-remote-project-create`, `muggle-remote-project-list` |
| Create/list use cases | `muggle-remote-use-case-create-from-prompts`, `muggle-remote-use-case-list` |
| Create/list test cases | `muggle-remote-test-case-create`, `muggle-remote-test-case-list` |
| Get test case details | `muggle-remote-test-case-get` |
| Get test script details | `muggle-remote-test-script-get` |

### Authentication

| You want to... | Use this tool |
| :------------- | :------------ |
| Check auth status | `muggle-remote-auth-status` |
| Log in | `muggle-remote-auth-login` |
| Poll for login completion | `muggle-remote-auth-poll` |
| Log out | `muggle-remote-auth-logout` |

---

## Next Steps

- **[Agent Skills](skills.md)** — Pre-built workflows that automate multi-step testing
- **[Example Workflows](examples.md)** — See these tools in action
- **[Local Testing Setup](setup.md)** — Installation and configuration
- **[Local Testing Overview](overview.md)** — Understanding the architecture
- **[Cloud QA Tools](../mcp/mcp-api-reference.md)** — Full reference for muggle-remote-* tools
