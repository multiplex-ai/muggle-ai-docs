# Local Testing Tools Reference

Complete reference for local testing tools in the `@muggleai/mcp` package.

> **Architecture Note:** Local testing uses a **cloud-first** approach:
> - **`qa_*` tools**: Authentication and all entity management (projects, use cases, test cases, secrets)
> - **`muggle_*` tools**: Local execution, results viewing, and publishing only

## Tool Namespaces

| Namespace | Purpose | Examples |
| :-------- | :------ | :------- |
| `qa_auth_*` | Authentication | `qa_auth_login`, `qa_auth_status` |
| `qa_*` | Cloud entity management | `qa_project_create`, `qa_test_case_get` |
| `muggle_*` | Local execution & results | `muggle_execute_test_generation` |

## Local Tool Categories

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Status** | `muggle_check_status`, `muggle_list_sessions` | Check local service status |
| **Execution** | `muggle_execute_test_generation`, `muggle_execute_replay` | Run test generation and replay |
| **Results** | `muggle_run_result_list`, `muggle_run_result_get` | View execution results |
| **Scripts** | `muggle_test_script_list`, `muggle_test_script_get` | View locally generated scripts |
| **Publishing** | `muggle_publish_test_script` | Upload generated scripts to cloud |
| **Control** | `muggle_cancel_execution` | Cancel running executions |

---

## Authentication Tools (qa_auth_*)

Authentication tools are in the `qa_*` namespace because they interact with the cloud.

### qa_auth_status

Check current authentication status.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `authenticated` - Whether logged in
- `email` - User email (if authenticated)
- `expiresAt` - Token expiration time

---

### qa_auth_login

Start Device Code authentication flow.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `waitForCompletion` | boolean | No | Wait for browser login (default: true) |
| `timeoutMs` | number | No | Timeout in milliseconds (default: 120000) |

**Returns:**
- `verificationUri` - URL to open in browser
- `userCode` - Code to enter
- `deviceCode` - Code for polling (use with `qa_auth_poll`)

**Example prompt:**
> "Log in to Muggle Test"

---

### qa_auth_poll

Poll for Device Code authentication completion.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `deviceCode` | string | No | Device code from `qa_auth_login` |

**Returns:**
- `status` - `complete`, `pending`, `expired`, or `error`
- `email` - User email (if complete)

---

### qa_auth_logout

Clear stored credentials.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

---

## Status Tools

### muggle_check_status

Check the status of Muggle Test Local service.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `dataDirectory` - Local data storage path
- `sessionsDirectory` - Sessions storage path
- `authenticated` - Whether authenticated

---

### muggle_list_sessions

List all stored testing sessions.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `limit` | number | No | Maximum sessions to return (default: 10) |

---

## Execution Tools

### muggle_execute_test_generation

Generate a test script by running browser automation locally.

**Important:** First call `qa_test_case_get` to fetch test case details, then pass them here.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testCase` | object | Yes | Test case details from `qa_test_case_get` |
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
1. qa_test_case_get(testCaseId) → returns test case object
2. muggle_execute_test_generation({ testCase: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle_execute_replay

Replay a test script locally.

**Important:** First call `qa_test_script_get` to fetch script details, then pass them here.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testScript` | object | Yes | Test script details from `qa_test_script_get` |
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
1. qa_test_script_get(testScriptId) → returns test script object
2. muggle_execute_replay({ testScript: {...}, localUrl: "http://localhost:3000", approveElectronAppLaunch: true })
```

---

### muggle_cancel_execution

Cancel a running execution.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run ID to cancel |

---

## Run Result Tools

### muggle_run_result_list

List execution results.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | No | Filter by cloud test case ID |
| `limit` | number | No | Maximum results (default: 20) |

---

### muggle_run_result_get

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

### muggle_test_script_list

List locally generated test scripts.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudTestCaseId` | string | No | Filter by cloud test case ID |

---

### muggle_test_script_get

Get test script details including action script steps.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `testScriptId` | string | Yes | Local test script ID |

---

## Publishing Tools

### muggle_publish_test_script

Upload a locally generated test script to the cloud.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run result ID from test generation |
| `cloudTestCaseId` | string | Yes | Cloud test case ID to publish under |

**Requires:** Authentication via `qa_auth_login`

---

## Workflow: Local Testing with Cloud Entities

The recommended workflow uses cloud tools (`qa_*`) to manage entities and local tools (`muggle_*`) to execute:

```
┌─────────────────────────────────────────────────────────────┐
│                     Entity Management                        │
│                      (qa_* tools)                            │
├─────────────────────────────────────────────────────────────┤
│  qa_auth_login          → Authenticate                       │
│  qa_project_list        → Find/create project                │
│  qa_use_case_list       → Find/create use case               │
│  qa_test_case_list      → Find/create test case              │
│  qa_test_case_get       → Fetch test case details            │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                     Local Execution                          │
│                    (muggle_* tools)                          │
├─────────────────────────────────────────────────────────────┤
│  muggle_execute_test_generation → Generate script locally    │
│  muggle_run_result_get          → View results               │
│  muggle_publish_test_script     → Publish back to cloud      │
└─────────────────────────────────────────────────────────────┘
```

### Example: Test Login Feature on localhost:3000

```
1. qa_auth_status                         # Check auth
2. qa_auth_login                          # Login if needed
3. qa_project_list                        # Find project
4. qa_use_case_list(projectId)            # Find login use case
5. qa_test_case_list_by_use_case(ucId)    # Find login test case
6. qa_test_case_get(testCaseId)           # Get full details
7. [Ask user for approval]
8. muggle_execute_test_generation({
     testCase: { ...from step 6... },
     localUrl: "http://localhost:3000",
     approveElectronAppLaunch: true
   })
9. muggle_run_result_get(runId)           # View results
10. muggle_publish_test_script(runId, testCaseId)  # Publish
```

---

## Tool Selection Guide

### Test Execution

| You want to... | Use this flow |
| :------------- | :------------ |
| Generate a test script | `qa_test_case_get` → `muggle_execute_test_generation` |
| Replay an existing script | `qa_test_script_get` → `muggle_execute_replay` |
| Stop a running test | `muggle_cancel_execution` |
| View test results | `muggle_run_result_get` |

### Entity Management (use qa_* tools)

| You want to... | Use this tool |
| :------------- | :------------ |
| Create/list projects | `qa_project_create`, `qa_project_list` |
| Create/list use cases | `qa_use_case_create_from_prompts`, `qa_use_case_list` |
| Create/list test cases | `qa_test_case_create`, `qa_test_case_list` |
| Get test case details | `qa_test_case_get` |
| Get test script details | `qa_test_script_get` |

### Authentication

| You want to... | Use this tool |
| :------------- | :------------ |
| Check auth status | `qa_auth_status` |
| Log in | `qa_auth_login` |
| Poll for login completion | `qa_auth_poll` |
| Log out | `qa_auth_logout` |

---

## Next Steps

- **[Agent Skills](skills.md)** — Pre-built workflows that automate multi-step testing
- **[Example Workflows](examples.md)** — See these tools in action
- **[Local Testing Setup](setup.md)** — Installation and configuration
- **[Local Testing Overview](overview.md)** — Understanding the architecture
- **[Cloud QA Tools](../mcp/mcp-api-reference.md)** — Full reference for qa_* tools
