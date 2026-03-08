# Local Testing Tools Reference

Complete reference for all Muggle Test Local MCP tools.

## Tool Categories

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Test Execution** | `muggle_run_test`, `muggle_explore_page`, `muggle_execute_action` | Run tests and interact with pages |
| **Observation** | `muggle_get_screenshot`, `muggle_get_page_state` | Capture current state |
| **Session Management** | `muggle_list_sessions`, `muggle_cleanup_sessions`, `muggle_check_status` | Manage test sessions |
| **Authentication** | `muggle_auth_status`, `muggle_auth_login`, `muggle_auth_logout` | Optional authentication |

---

## Test Execution Tools

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

| You want to... | Use this tool |
| :------------- | :------------ |
| Run a complete test scenario | `muggle_run_test` |
| See what's on a page | `muggle_explore_page` |
| Click a specific button | `muggle_execute_action` (action_type: click) |
| Fill in a form field | `muggle_execute_action` (action_type: type) |
| Take a picture of the page | `muggle_get_screenshot` |
| Check current URL | `muggle_get_page_state` |
| Find previous test results | `muggle_list_sessions` |
| Free up disk space | `muggle_cleanup_sessions` |
| Verify setup is working | `muggle_check_status` |

## Next Steps

- **[Example Workflows](local-testing/examples.md)** — See these tools in action
- **[Local Testing Setup](local-testing/setup.md)** — Installation and configuration
- **[Local Testing Overview](local-testing/overview.md)** — Understanding the architecture
