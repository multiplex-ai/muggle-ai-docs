# Local Testing Tools Reference

Complete reference for all Muggle Test Local MCP tools.

## Tool Categories

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Project Management** | `muggle_project_*` | Create and manage local test projects |
| **Use Case Management** | `muggle_use_case_*` | Define user flows and scenarios |
| **Test Case Management** | `muggle_test_case_*` | Specify individual test cases |
| **Test Script Management** | `muggle_test_script_*` | Manage generated test scripts |
| **Test Execution** | `muggle_execute_*`, `muggle_run_test` | Generate and replay test scripts |
| **Run Results** | `muggle_run_result_*` | View execution history |
| **Publishing** | `muggle_publish_*` | Sync local projects to cloud |
| **Browser Interaction** | `muggle_explore_page`, `muggle_execute_action` | Direct browser control |
| **Observation** | `muggle_get_screenshot`, `muggle_get_page_state` | Capture current state |
| **Session Management** | `muggle_list_sessions`, `muggle_cleanup_sessions`, `muggle_check_status` | Manage test sessions |
| **Authentication** | `muggle_auth_*` | Optional Muggle AI authentication |

---

## Project Management Tools

Manage local test projects stored in `~/.muggle-ai/projects/`.

### muggle_project_create

Create a new local test project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `name` | string | Yes | Project name |
| `url` | string | Yes | Target URL (supports localhost) |
| `description` | string | No | Project description |

**Example prompt**:
> "Create a local project called 'My App Tests' for localhost:3000"

---

### muggle_project_list

List all local projects.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns**: List of projects with IDs, names, URLs, and timestamps.

---

### muggle_project_get

Get details of a specific project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |

---

### muggle_project_update

Update project properties.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `name` | string | No | Updated name |
| `url` | string | No | Updated URL |
| `description` | string | No | Updated description |

---

### muggle_project_delete

Delete a local project and all its contents.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID to delete |

---

## Use Case Management Tools

Define user flows and scenarios within a project.

### muggle_use_case_save

Save a use case to a project.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Parent project ID |
| `useCase.title` | string | Yes | Use case title |
| `useCase.userStory` | string | No | User story description |
| `useCase.description` | string | No | Detailed description |
| `useCase.breakdownItems` | array | No | Step breakdown |

**Example prompt**:
> "Create a use case for 'User Registration' in my project"

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
| `title` | string | No | Updated title |
| `userStory` | string | No | Updated user story |
| `description` | string | No | Updated description |
| `breakdownItems` | array | No | Updated breakdown |

---

### muggle_use_case_delete

Delete a use case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Use case ID |

---

## Test Case Management Tools

Define specific test scenarios within use cases.

### muggle_test_case_save

Save a test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Parent use case ID |
| `testCase.title` | string | Yes | Test case title |
| `testCase.description` | string | Yes | Test description |
| `testCase.goal` | string | Yes | Test goal |
| `testCase.precondition` | string | No | Prerequisites |
| `testCase.instructions` | string | No | Step-by-step instructions |
| `testCase.expectedResult` | string | Yes | Expected outcome |
| `testCase.url` | string | Yes | Target URL |

**Example prompt**:
> "Add a test case for successful login to the User Registration use case"

---

### muggle_test_case_list

List test cases.

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
| `title` | string | No | Updated title |
| `description` | string | No | Updated description |
| `goal` | string | No | Updated goal |
| `precondition` | string | No | Updated precondition |
| `instructions` | string | No | Updated instructions |
| `expectedResult` | string | No | Updated expected result |
| `url` | string | No | Updated URL |

---

### muggle_test_case_delete

Delete a test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case ID |

---

## Test Script Management Tools

Manage generated test scripts (action scripts with screenshots).

### muggle_test_script_save

Manually save a test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `useCaseId` | string | Yes | Use case ID |
| `testCaseId` | string | Yes | Test case ID |
| `testScript.name` | string | Yes | Script name |
| `testScript.url` | string | Yes | Target URL |
| `testScript.actionScript` | array | No | Action steps |
| `testScript.actionScriptId` | string | No | Cloud action script ID |

> **Note**: Test scripts are typically created automatically by `muggle_execute_test_generation`.

---

### muggle_test_script_list

List test scripts.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | No | Filter by test case |

---

### muggle_test_script_get

Get test script details including action steps.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Test script ID |

---

### muggle_test_script_delete

Delete a local test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Test script ID |

> **Note**: Deleting locally does not affect cloud copies.

---

## Execution Tools

Generate and replay test scripts using the Muggle AI Studio (electron-app).

### muggle_execute_test_generation

Generate a test script from a test case.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testCaseId` | string | Yes | Test case to generate script for |
| `timeoutMs` | number | No | Timeout (default: 300000 = 5 min) |

**What happens**:
1. Launches Muggle AI Studio in "explore" mode
2. Navigates to the test case URL
3. AI generates action steps to achieve the test goal
4. Screenshots are captured at each step
5. Action script is saved locally and uploaded to Firebase
6. Run result is recorded

**Example prompt**:
> "Generate a test script for the login test case"

---

### muggle_execute_replay

Replay a generated test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | Yes | Test script to replay |
| `timeoutMs` | number | No | Timeout (default: 180000 = 3 min) |

**What happens**:
1. Launches Muggle AI Studio in "engine" mode
2. Executes the recorded action steps
3. Verifies expected outcomes
4. Records pass/fail result

---

### muggle_cancel_execution

Cancel a running test execution.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `runId` | string | Yes | Run ID to cancel |

---

## Run Result Tools

View execution history and results.

### muggle_run_result_list

List execution history.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `testScriptId` | string | No | Filter by test script |
| `limit` | number | No | Max results (default: 20) |

**Returns**: List of runs with type, status, duration, and timestamps.

---

### muggle_run_result_get

Get detailed run result.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Project ID |
| `runId` | string | Yes | Run ID |

**Returns**: Full run details including screenshots and action script output.

---

## Publishing Tools

Sync local projects to Muggle AI cloud for team collaboration and scheduled runs.

### muggle_publish_project

Publish a local project to the cloud.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Local project ID |
| `targetUrl` | string | No | Production URL (updates project URL) |
| `cloudProjectId` | string | No | Existing cloud project to update |

**What happens**:
1. Creates/updates cloud project
2. Syncs all use cases
3. Syncs all test cases
4. Records cloud ID mappings locally

**Example prompt**:
> "Publish my local project to the cloud with production URL https://myapp.com"

---

### muggle_publish_test_script

Check cloud status of a test script.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `projectId` | string | Yes | Local project ID |
| `testScriptId` | string | Yes | Test script ID |
| `cloudProjectId` | string | Yes | Cloud project ID |
| `cloudUseCaseId` | string | Yes | Cloud use case ID |
| `cloudTestCaseId` | string | Yes | Cloud test case ID |

> **Note**: Test scripts are automatically uploaded to Firebase during `muggle_execute_test_generation`. This tool verifies the upload status.

---

## Browser Interaction Tools

### muggle_run_test

Execute an AI-driven test flow against a web application.

**When to use**: For comprehensive testing scenarios where you describe what to test in natural language.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `url` | string | Yes | Target URL (supports localhost) |
| `instructions` | string | Yes | Natural language test instructions |
| `timeout_ms` | number | No | Maximum execution time (default: 60000) |
| `capture_screenshots` | boolean | No | Include screenshots (default: true) |

**Example prompt**:
> "Run a test on localhost:3000/checkout. Add an item to the cart, proceed to checkout, and verify the total is calculated correctly."

**Returns**:
- Test status (passed/failed/error/timeout)
- Step-by-step results
- Screenshots of each step
- Session path for detailed review

---

### muggle_explore_page

Navigate to a URL and analyze the page structure.

**When to use**: To understand what elements are available before writing detailed test instructions.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `url` | string | Yes | Target URL to explore |
| `analysis_depth` | string | No | `quick`, `standard`, or `detailed` (default: standard) |

**Example prompt**:
> "Explore the page at localhost:3000/dashboard and tell me what interactive elements are available."

**Returns**:
- Page title and final URL
- List of interactive elements (buttons, links, inputs)
- Detected forms with field descriptions
- Suggested next actions
- Screenshot of the page

---

### muggle_execute_action

Execute a single browser action on the current page.

**When to use**: For step-by-step control when you need precise actions.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `action_type` | string | Yes | One of: `click`, `type`, `select`, `scroll`, `navigate`, `wait` |
| `target` | string | Conditional | Element selector or description (for click/type/select) |
| `value` | string | Conditional | Text to type or option to select |
| `url` | string | Conditional | URL to navigate to (for navigate action) |
| `duration_ms` | number | Conditional | Wait duration in milliseconds (for wait action) |

**Example prompts**:
> "Click the 'Sign In' button"

> "Type 'user@example.com' into the email field"

> "Select 'Express Shipping' from the shipping options dropdown"

**Returns**:
- Success/failure status
- Description of action performed
- Whether page navigation occurred
- Screenshot after action

---

## Observation Tools

### muggle_get_screenshot

Capture the current page as a screenshot.

**When to use**: To see the current visual state of the page.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `full_page` | boolean | No | Capture full scrollable page (default: false) |
| `execution_id` | string | No | Specific session to capture (uses current if not provided) |

**Example prompt**:
> "Take a screenshot of the current page"

**Returns**:
- Path to saved screenshot
- Current page URL
- Viewport dimensions

---

### muggle_get_page_state

Get the current browser state without taking any action.

**When to use**: To check where the browser is currently navigated.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `executionId` | string | No | Specific session to query (uses current if not provided) |

**Example prompt**:
> "Where am I on the site right now?"

**Returns**:
- Current URL
- Page title
- Whether a session is active
- Session path

---

## Session Management Tools

### muggle_list_sessions

List all stored testing sessions.

**When to use**: To review past test runs or find specific session results.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `limit` | number | No | Maximum sessions to return (default: 10) |

**Example prompt**:
> "Show me my recent testing sessions"

**Returns**:
- List of sessions with:
  - Session ID
  - Status (running/completed/failed)
  - Start time
  - Duration
  - Target URL
  - Number of steps

---

### muggle_cleanup_sessions

Delete old testing sessions to free disk space.

**When to use**: When sessions are accumulating and using disk space.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| `max_age_days` | number | No | Delete sessions older than this (default: 30) |

**Example prompt**:
> "Clean up testing sessions older than 7 days"

**Returns**:
- Number of sessions deleted
- Space freed (approximate)

---

### muggle_check_status

Check the connection status to the web service.

**When to use**: To verify the local testing infrastructure is running correctly.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Example prompt**:
> "Check if Muggle Test Local is working"

**Returns**:
- Connection status (healthy/unhealthy)
- Web service URL
- Current session info (if any)

---

## Authentication Tools

> **Note**: Authentication is optional for local testing. These tools are only needed if you want to sync with cloud features.

### muggle_auth_status

Check current authentication status.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns**:
- Whether authenticated
- Email (if authenticated)
- Token expiration time

---

### muggle_auth_login

Authenticate with the service.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns**:
- Success status
- Authentication details

---

### muggle_auth_logout

Clear stored credentials.

| Parameter | Type | Required | Description |
| :-------- | :--- | :------: | :---------- |
| (none) | — | — | — |

**Returns**:
- Success status

---

## Tool Selection Guide

### Project Setup

| You want to... | Use this tool |
| :------------- | :------------ |
| Start a new test project | `muggle_project_create` |
| See all my projects | `muggle_project_list` |
| Define a user flow | `muggle_use_case_save` |
| Create a test scenario | `muggle_test_case_save` |

### Test Execution

| You want to... | Use this tool |
| :------------- | :------------ |
| Generate a test script from a test case | `muggle_execute_test_generation` |
| Replay an existing test script | `muggle_execute_replay` |
| Run a quick ad-hoc test | `muggle_run_test` |
| Stop a running test | `muggle_cancel_execution` |

### Browser Interaction

| You want to... | Use this tool |
| :------------- | :------------ |
| See what's on a page | `muggle_explore_page` |
| Click a specific button | `muggle_execute_action` (action_type: click) |
| Fill in a form field | `muggle_execute_action` (action_type: type) |
| Take a picture of the page | `muggle_get_screenshot` |
| Check current URL | `muggle_get_page_state` |

### Results and History

| You want to... | Use this tool |
| :------------- | :------------ |
| See test execution history | `muggle_run_result_list` |
| View detailed run results | `muggle_run_result_get` |
| List generated test scripts | `muggle_test_script_list` |
| Find previous sessions | `muggle_list_sessions` |

### Publishing

| You want to... | Use this tool |
| :------------- | :------------ |
| Share project with team | `muggle_publish_project` |
| Check if script uploaded | `muggle_publish_test_script` |

### Maintenance

| You want to... | Use this tool |
| :------------- | :------------ |
| Verify setup is working | `muggle_check_status` |
| Free up disk space | `muggle_cleanup_sessions` |
| Delete a project | `muggle_project_delete` |

## Next Steps

- **[Example Workflows](local-testing/examples.md)** — See these tools in action
- **[Local Testing Setup](local-testing/setup.md)** — Installation and configuration
- **[Local Testing Overview](local-testing/overview.md)** — Understanding the architecture
