# Local Testing Tools Reference

Complete reference for local testing tools in the `@muggleai/mcp` package.

> **Note:** All local testing tools use the `muggle_` prefix. Cloud QA tools use the `qa_` prefix. When running `muggle-mcp serve` (default), both sets of tools are available.

## Tool Categories

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Authentication** | `muggle_auth_*` | Login, logout, check auth status |
| **Project Management** | `muggle_project_*` | Create and manage test projects |
| **Use Cases** | `muggle_use_case_*` | Define user flows and scenarios |
| **Test Cases** | `muggle_test_case_*` | Detailed test specifications |
| **Test Scripts** | `muggle_test_script_*` | Generated automation scripts |
| **Execution** | `muggle_execute_*` | Run test generation and replay |
| **Results** | `muggle_run_result_*` | View execution results |
| **Publishing** | `muggle_publish_*` | Upload to cloud |
| **Cloud Pull** | `muggle_cloud_*` | List and pull from cloud |

---

## Authentication Tools

### muggle_auth_status

Check current authentication status.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `authenticated` - Whether logged in
- `email` - User email (if authenticated)
- `expiresAt` - Token expiration time

---

### muggle_auth_login

Start Device Code authentication flow.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns:**
- `verificationUri` - URL to open in browser
- `userCode` - Code to enter
- `deviceCode` - Code for polling (use with `muggle_auth_poll`)
- `expiresIn` - Seconds until code expires

**Example prompt:**
> "Log in to Muggle Test"

---

### muggle_auth_poll

Poll for Device Code authentication completion.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `deviceCode` | string | Yes | Device code from `muggle_auth_login` |

**Returns:**
- `status` - `complete`, `pending`, `expired`, or `error`
- `email` - User email (if complete)

---

### muggle_auth_logout

Clear stored credentials.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

---

## Project Management Tools

### muggle_project_create

Create a new local test project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `name` | string | Yes | Project name |
| `url` | string | Yes | Base URL of the application |
| `description` | string | No | Project description |

**Example prompt:**
> "Create a project for my app at localhost:3000"

**Returns:**
- `projectId` - Generated project ID
- `path` - Local storage path

---

### muggle_project_list

List all local projects.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

---

### muggle_project_get

Get details of a specific project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |

---

### muggle_project_update

Update project details.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `name` | string | No | New name |
| `url` | string | No | New URL |
| `description` | string | No | New description |

---

### muggle_project_delete

Delete a project and all its contents.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |

---

## Use Case Tools

### muggle_use_case_save

Create a new use case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Parent project ID |
| `useCase` | object | Yes | Use case definition |

**Use case object:**
- `title` - Use case title
- `userPersona` - Who is performing the action
- `goal` - What the user wants to achieve
- `breakdownItems` - Step-by-step breakdown

**Example prompt:**
> "Create a use case for user login flow"

---

### muggle_use_case_list

List use cases in a project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |

---

### muggle_use_case_get

Get use case details.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Use case ID |

---

### muggle_use_case_update

Update a use case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Use case ID |
| `useCase` | object | Yes | Updated use case |

---

### muggle_use_case_delete

Delete a use case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Use case ID |

---

## Test Case Tools

### muggle_test_case_save

Create a new test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Parent project ID |
| `useCaseId` | string | No | Parent use case ID |
| `testCase` | object | Yes | Test case definition |

**Test case object:**
- `title` - Test case title
- `goal` - What the test verifies
- `url` - Starting URL
- `precondition` - Setup requirements
- `steps` - Test steps
- `expectedResult` - Expected outcome

---

### muggle_test_case_list

List test cases in a project or use case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | No | Filter by use case |

---

### muggle_test_case_get

Get test case details.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case ID |

---

### muggle_test_case_update

Update a test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case ID |
| `testCase` | object | Yes | Updated test case |

---

### muggle_test_case_delete

Delete a test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case ID |

---

## Test Script Tools

### muggle_test_script_save

Save a generated test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Parent test case ID |
| `actionScript` | object | Yes | Script definition |

---

### muggle_test_script_list

List test scripts in a project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | No | Filter by test case |

---

### muggle_test_script_get

Get test script details.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Test script ID |

---

### muggle_test_script_delete

Delete a test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Test script ID |

---

## Execution Tools

### muggle_execute_test_generation

Generate a test script by running the browser automation.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case to generate script for |

**What happens:**
1. Browser engine launches
2. Navigates to the test case URL
3. AI explores and records actions
4. Script is saved to the project

**Example prompt:**
> "Generate a test script for the login test case"

---

### muggle_execute_replay

Replay a test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Script to replay |

**Returns:**
- `status` - Pass/fail result
- `executionTime` - Duration
- `screenshots` - Captured screenshots

---

### muggle_cancel_execution

Cancel a running execution.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `executionId` | string | Yes | Execution to cancel |

---

## Run Result Tools

### muggle_run_result_list

List execution results.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | No | Filter by script |

---

### muggle_run_result_get

Get execution result details.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `runId` | string | Yes | Run ID |

---

## Publishing Tools

### muggle_publish_project

Upload a project to the Muggle AI cloud.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project to publish |
| `targetUrl` | string | No | Override URL for cloud execution |

**Requires:** Authentication

---

### muggle_publish_test_script

Upload a single test script to the cloud.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Script to publish |

**Requires:** Authentication

---

## Cloud Pull Tools

### muggle_cloud_project_list

List all cloud projects for the authenticated user.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Requires:** Authentication

**Returns:**
- `projects` - Array of cloud projects with IDs, names, URLs

---

### muggle_cloud_pull_project

Pull an entire cloud project to local storage with URL rewriting.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudProjectId` | string | Yes | Cloud project ID to pull |
| `localUrl` | string | Yes | Local URL for testing (e.g., `http://localhost:3000`) |

**What happens:**
1. Downloads project metadata from cloud
2. Creates local project with `localUrl`
3. Stores original cloud URL in `originalUrl` field
4. Downloads and creates all use cases
5. Downloads and creates all test cases with URL rewriting
6. Maps cloud IDs to local IDs for future sync

**URL Rewriting:**
- Path is preserved: `https://app.example.com/login` → `http://localhost:3000/login`
- Original URL stored in `originalUrl` for publishing back

**Requires:** Authentication

---

### muggle_cloud_pull_use_case

Pull a single cloud use case and its test cases to local storage.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudProjectId` | string | Yes | Cloud project ID |
| `cloudUseCaseId` | string | Yes | Cloud use case ID to pull |
| `localProjectId` | string | Yes | Local project to save under |
| `localUrl` | string | Yes | Local URL for testing |

**Requires:** Authentication

---

### muggle_cloud_pull_test_case

Pull a single cloud test case to local storage.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `cloudProjectId` | string | Yes | Cloud project ID |
| `cloudUseCaseId` | string | Yes | Cloud use case ID |
| `cloudTestCaseId` | string | Yes | Cloud test case ID to pull |
| `localProjectId` | string | Yes | Local project to save under |
| `localUseCaseId` | string | Yes | Local use case to save under |
| `localUrl` | string | Yes | Local URL for testing |

**Requires:** Authentication

---

## Tool Selection Guide

### Project Setup

| You want to... | Use this tool |
| :------------- | :------------ |
| Start a new test project | `muggle_project_create` |
| See all my local projects | `muggle_project_list` |
| See all my cloud projects | `muggle_cloud_project_list` |
| Pull a project from cloud | `muggle_cloud_pull_project` |
| Define a user flow | `muggle_use_case_save` |
| Create a test scenario | `muggle_test_case_save` |

### Test Execution

| You want to... | Use this tool |
| :------------- | :------------ |
| Generate a test script from a test case | `muggle_execute_test_generation` |
| Replay an existing test script | `muggle_execute_replay` |
| Run a quick ad-hoc test | `muggle_run_test` |
| Stop a running test | `muggle_cancel_execution` |

### Cloud Sync

| You want to... | Use this tool |
| :------------- | :------------ |
| List cloud projects | `muggle_cloud_project_list` |
| Pull project from cloud to local | `muggle_cloud_pull_project` |
| Pull a single use case | `muggle_cloud_pull_use_case` |
| Pull a single test case | `muggle_cloud_pull_test_case` |
| Publish local to cloud | `muggle_publish_project` |

### Common Workflow

| You want to... | Use this tool |
| :------------- | :------------ |
| Start a new testing project | `muggle_project_create` |
| Define a user flow | `muggle_use_case_save` |
| Create a specific test | `muggle_test_case_save` |
| Generate automation script | `muggle_execute_test_generation` |
| Run a test | `muggle_execute_replay` |
| View test results | `muggle_run_result_get` |
| Upload to cloud | `muggle_publish_project` |
| Log in | `muggle_auth_login` |

## Next Steps

- **[Agent Skills](skills.md)** — Pre-built workflows that automate multi-step testing
- **[Example Workflows](examples.md)** — See these tools in action
- **[Local Testing Setup](setup.md)** — Installation and configuration
- **[Local Testing Overview](overview.md)** — Understanding the architecture
