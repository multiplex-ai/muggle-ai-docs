# MCP API Reference

Complete reference for Cloud QA tools in the `@muggleai/mcp` package.

> **Note:** This page documents Cloud QA tools (prefix: `qa_`). For local testing tools (prefix: `muggle_`), see [Local Testing Tools Reference](../local-testing/tools-reference.md).

## Overview

The Cloud QA tools are organized into these categories:

| Category        | Count | Purpose                            | Link                          |
| :-------------- | ----: | :--------------------------------- | :---------------------------- |
| Authentication  |     7 | Authenticate and manage API keys   | [Jump](#authentication-tools) |
| Project         |     5 | Create and manage testing projects | [Jump](#project-tools)        |
| PRD Files       |     5 | Upload and process PRD documents   | [Jump](#prd-file-tools)       |
| Secrets         |     5 | Manage test credentials            | [Jump](#secret-tools)         |
| Wallet          |     5 | Manage payment methods and topups  | [Jump](#wallet-tools)         |
| Use Cases       |     7 | Discover, create, and update use cases | [Jump](#use-case-tools)   |
| Workflows       |    17 | Execute testing workflows          | [Jump](#workflow-tools)       |
| Artifacts       |    11 | Inspect test cases and scripts     | [Jump](#artifact-tools)       |
| Reports         |     4 | Generate and deliver reports       | [Jump](#report-tools)         |
| Recommendations |     2 | Get scheduling guidance            | [Jump](#recommendation-tools) |

---

## Authentication Tools

These tools enable **agentic authentication** - AI agents can help users authenticate without needing a pre-existing API key.

> **Security Note:** API keys obtained through the device code flow are written directly to the user's local config file. They are **never** returned through the MCP protocol to prevent leakage to AI agents or conversation history.

### MCP Local Auth Tools (qa_auth_*)

These tools are available when running `muggle-mcp serve` and are used for local testing authentication:

| Tool | Description |
| :--- | :---------- |
| `qa_auth_status` | Check current authentication status |
| `qa_auth_login` | Start device code authentication flow |
| `qa_auth_poll` | Poll for login completion |
| `qa_auth_logout` | Clear stored credentials |

See [Local Testing Tools Reference](../local-testing/tools-reference.md#authentication-tools-qa_auth_) for full documentation.

### Gateway Auth Tools

### Device Code Flow

The device code flow allows users to authenticate through their browser:

```mermaid
sequenceDiagram
    participant Agent as AI Agent
    participant Gateway as MCP Gateway
    participant User as User (Browser)
    participant Auth0 as Auth0
    
    Agent->>Gateway: auth_device_code_start()
    Gateway->>Auth0: Request device code
    Auth0-->>Gateway: {userCode, verificationUri}
    Gateway-->>Agent: "Visit URL, enter code"
    Agent->>User: Display URL and code
    User->>Auth0: Visit URL, enter code, login
    Auth0-->>Auth0: User authorizes
    Agent->>Gateway: auth_device_code_poll(deviceCode)
    Gateway->>Auth0: Check authorization
    Auth0-->>Gateway: Access token
    Gateway->>Gateway: Create API key
    Gateway->>Gateway: Write to ~/.cursor/mcp.json
    Gateway-->>Agent: "Success! Restart Cursor"
```

### auth_status

Check current authentication status.

**Input:** None required

**Output:**

```json
{
  "authenticated": true,
  "method": "apiKey",
  "apiKeyHint": "mai_sk_abc...xyz",
  "message": "Authenticated via API key"
}
```

| Field          | Type    | Description                          |
| :------------- | :------ | :----------------------------------- |
| `authenticated`| boolean | Whether credentials are present      |
| `method`       | string  | `"apiKey"`, `"bearer"`, or `"none"`  |
| `apiKeyHint`   | string  | Masked API key (if using API key)    |
| `message`      | string  | Human-readable status message        |

### auth_device_code_start

Start the device code authentication flow. Returns a URL for the user to visit.

**Input:** None required

**Output:**

```json
{
  "userCode": "ABCD-EFGH",
  "verificationUri": "https://login.muggle-ai.com/activate",
  "verificationUriComplete": "https://login.muggle-ai.com/activate?user_code=ABCD-EFGH",
  "expiresIn": 900,
  "interval": 5,
  "instructions": [
    "1. Open this URL in your browser: https://login.muggle-ai.com/activate",
    "2. Enter this code: ABCD-EFGH",
    "3. Log in with your Muggle AI account",
    "4. Once authorized, call auth_device_code_poll to complete setup"
  ]
}
```

### auth_device_code_poll

Poll for device code authorization completion. Call this after the user has visited the verification URL.

**Input:**

| Field        | Type   | Required | Description                       |
| :----------- | :----- | :------: | :-------------------------------- |
| `deviceCode` | string |    ✓     | Device code from start response   |

**Output (pending):**

```json
{
  "status": "authorization_pending",
  "message": "Waiting for user to authorize..."
}
```

**Output (success):**

```json
{
  "success": true,
  "status": "authorized",
  "message": "Authentication successful! API key created.",
  "apiKey": {
    "id": "key_abc123",
    "hint": "mai_sk_abc...xyz",
    "expiresAt": "2024-04-15T00:00:00Z"
  },
  "mcpConfigUpdated": true,
  "mcpConfigPath": "C:\\Users\\you\\.cursor\\mcp.json",
  "instructions": [
    "Your Cursor mcp.json has been automatically updated!",
    "NEXT STEP: Restart Cursor to load the new configuration.",
    "SECURITY: The full API key is NOT shown here - it went directly to your config file."
  ]
}
```

### auth_api_key_create

Create a new API key for the authenticated user.

**Input:**

| Field            | Type    | Required | Description                                           |
| :--------------- | :------ | :------: | :---------------------------------------------------- |
| `name`           | string  |          | Name for the API key                                  |
| `expiry`         | string  |          | `"30d"`, `"90d"`, `"1y"`, or `"never"` (default: 90d) |
| `updateMcpConfig`| boolean |          | Auto-update mcp.json (default: true)                  |

**Output:**

```json
{
  "success": true,
  "apiKey": {
    "id": "key_abc123",
    "hint": "mai_sk_abc...xyz",
    "name": "My Dev Key",
    "expiresAt": "2024-04-15T00:00:00Z"
  },
  "secureFilePath": "C:\\Users\\you\\.muggle-ai-api-key",
  "mcpConfigUpdated": true
}
```

### auth_api_key_list

List all API keys for the authenticated user.

**Output:**

```json
{
  "count": 2,
  "apiKeys": [
    {
      "id": "key_abc123",
      "hint": "mai_sk_abc...xyz",
      "name": "Production",
      "status": "active",
      "createdAt": "2024-01-15T10:00:00Z"
    }
  ]
}
```

### auth_api_key_get

Get details of a specific API key by ID.

| Field      | Type   | Required | Description    |
| :--------- | :----- | :------: | :------------- |
| `apiKeyId` | string |    ✓     | API key ID     |

### auth_api_key_revoke

Revoke an API key. The key will immediately stop working.

| Field      | Type   | Required | Description       |
| :--------- | :----- | :------: | :---------------- |
| `apiKeyId` | string |    ✓     | API key to revoke |

**Output:**

```json
{
  "success": true,
  "message": "API key revoked successfully"
}
```

---

## Project Tools

### qa_project_create

Create a new QA testing project.

**Input:**

```json
{
  "projectName": "My Website Tests",
  "description": "End-to-end tests for example.com",
  "url": "https://example.com"
}
```

| Field         | Type   | Required | Description                         |
| :------------ | :----- | :------: | :---------------------------------- |
| `projectName` | string |    ✓     | Name of the project (max 255 chars) |
| `description` | string |    ✓     | Project description                 |
| `url`         | string |    ✓     | Target website URL                  |

**Output:**

```json
{
  "id": "proj_abc123",
  "name": "My Website Tests",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

### qa_project_get

Get details of a specific project by ID.

| Field       | Type   | Required | Description            |
| :---------- | :----- | :------: | :--------------------- |
| `projectId` | string |    ✓     | Project ID to retrieve |

### qa_project_update

Update an existing project's details.

| Field         | Type   | Required | Description          |
| :------------ | :----- | :------: | :------------------- |
| `projectId`   | string |    ✓     | Project ID to update |
| `projectName` | string |          | New project name     |
| `description` | string |          | Updated description  |
| `url`         | string |          | Updated target URL   |

### qa_project_list

List all projects accessible to you with pagination.

| Field      | Type   | Required | Default | Description              |
| :--------- | :----- | :------: | :------ | :----------------------- |
| `page`     | number |          | 1       | Page number              |
| `pageSize` | number |          | 20      | Items per page (max 100) |

### qa_project_delete

Delete a project and all associated entities (use cases, test cases, test scripts). This is a soft delete - the project can be restored.

| Field       | Type   | Required | Description          |
| :---------- | :----- | :------: | :------------------- |
| `projectId` | string |    ✓     | Project ID to delete |

**Output:**

```json
{
  "id": "proj_abc123",
  "name": "My Website Tests",
  "deletedAt": "2024-01-15T12:00:00Z"
}
```

---

## PRD File Tools

### qa_prd_file_upload

Upload a Product Requirements Document to help AI generate accurate use cases.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "fileName": "requirements.pdf",
  "contentBase64": "JVBERi0xLjQKJ...",
  "contentType": "application/pdf"
}
```

| Field           | Type   | Required | Description                                     |
| :-------------- | :----- | :------: | :---------------------------------------------- |
| `projectId`     | string |    ✓     | Project to associate the file with              |
| `fileName`      | string |    ✓     | Name of the file                                |
| `contentBase64` | string |    ✓     | Base64-encoded file content                     |
| `contentType`   | string |          | MIME type (default: `application/octet-stream`) |

**Supported formats:**

| Format   | Extension | MIME Type                                                                 |
| :------- | :-------- | :------------------------------------------------------------------------ |
| PDF      | `.pdf`    | `application/pdf`                                                         |
| Word     | `.docx`   | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` |
| Text     | `.txt`    | `text/plain`                                                              |
| Markdown | `.md`     | `text/markdown`                                                           |

**Output:**

```json
{
  "message": "PRD file uploaded successfully.",
  "prdFilePath": "gs://muggle-ai.appspot.com/muggle-test/user_data/.../prd_file/requirements.pdf"
}
```

### qa_workflow_start_prd_file_process

Start processing an uploaded PRD file to extract use cases. This is an async workflow.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "name": "PRD Processing - Jan 2024",
  "description": "Extract use cases from Q1 requirements",
  "prdFilePath": "gs://muggle-ai.appspot.com/.../requirements.pdf",
  "originalFileName": "requirements.pdf",
  "url": "https://example.com"
}
```

| Field              | Type   | Required | Description                            |
| :----------------- | :----- | :------: | :------------------------------------- |
| `projectId`        | string |    ✓     | Target project                         |
| `name`             | string |    ✓     | Workflow name                          |
| `description`      | string |    ✓     | Description of the processing workflow |
| `prdFilePath`      | string |    ✓     | Storage path from upload response      |
| `originalFileName` | string |    ✓     | Original file name                     |
| `url`              | string |    ✓     | Target website URL for context         |

**Output:**

```json
{
  "workflowRuntime": {
    "id": "wfrt_prd_123",
    "status": "ACTIVE"
  },
  "prdFile": {
    "id": "prd_456",
    "fileName": "requirements.pdf"
  }
}
```

### qa_workflow_get_prd_file_process_latest_run

Check the status of a PRD file processing workflow.

| Field               | Type   | Required | Description         |
| :------------------ | :----- | :------: | :------------------ |
| `workflowRuntimeId` | string |    ✓     | Workflow runtime ID |

**Output:**

```json
{
  "status": "COMPLETED",
  "progress": 100,
  "updatedAt": "2024-01-15T10:35:00Z"
}
```

### qa_prd_file_list_by_project

List all PRD files for a project.

### qa_prd_file_delete

Delete a PRD file.

---

## Secret Tools

Secrets store credentials needed for testing. Values are encrypted and **never returned** in responses.

### qa_secret_create

Create a new secret.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "name": "TEST_USER_PASSWORD",
  "value": "secretpassword123",
  "description": "Password for test user account"
}
```

| Field         | Type   | Required | Description                      |
| :------------ | :----- | :------: | :------------------------------- |
| `projectId`   | string |    ✓     | Project to create secret for     |
| `name`        | string |    ✓     | Secret name/key                  |
| `value`       | string |    ✓     | Secret value (encrypted at rest) |
| `description` | string |          | Description of the secret        |

### qa_secret_list

List all secrets for a project.

> **Note:** Secret values are never returned in list responses.

### qa_secret_get

Get secret metadata (value is not returned).

### qa_secret_update

Update a secret's name, value, or description.

### qa_secret_delete

Delete a secret.

---

## Wallet Tools

Wallet tools help set up billing methods and top up credits.

### qa_wallet_payment_method_create_setup_session

Create a Stripe setup session to add a new saved payment method.

**Input:**

```json
{
  "checkoutSuccessCallback": "https://app.example.com/billing/payment-method/success",
  "checkoutCancelCallback": "https://app.example.com/billing/payment-method/cancel"
}
```

### qa_wallet_payment_method_list

List saved payment methods for the authenticated user.

**Input:**

```json
{}
```

### qa_wallet_auto_topup_set_payment_method

Set the saved payment method used for auto top-up.

**Input:**

```json
{
  "paymentMethodId": "pm_123"
}
```

### qa_wallet_auto_topup_update

Update auto-topup settings for threshold/package/enablement.

**Input:**

```json
{
  "enabled": true,
  "topUpTriggerTokenThreshold": 500,
  "packageId": "starter_pack"
}
```

### qa_wallet_topup

Create a one-time checkout session to purchase a token package.

**Input:**

```json
{
  "packageId": "starter_pack",
  "checkoutSuccessCallback": "https://app.example.com/billing/topup/success",
  "checkoutCancelCallback": "https://app.example.com/billing/topup/cancel"
}
```

---

## Use Case Tools

### qa_use_case_discovery_memory_get

Get discovered use case candidates with evidence (screenshots, notes).

**Output:**

```json
{
  "projectId": "proj_abc123",
  "useCaseCandidates": [
    {
      "id": "cand_001",
      "status": "candidate",
      "useCase": {
        "title": "User Login",
        "description": "Authenticate user with email and password"
      },
      "comments": [
        {
          "pageUrl": "https://example.com/login",
          "screenshotUrl": "https://storage.muggle-ai.com/screenshots/...",
          "notes": "Found login form with email and password fields"
        }
      ]
    }
  ]
}
```

**Candidate statuses:**

| Status      | Description                          |
| :---------- | :----------------------------------- |
| `draft`     | Initial discovery, not yet evaluated |
| `candidate` | Evaluated and ready for review       |
| `graduated` | Approved and promoted to use case    |
| `dropped`   | Rejected and archived                |

### qa_use_case_candidates_approve

Approve (graduate) selected candidates into active use cases.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "approvedCandidateIds": ["cand_001", "cand_002"]
}
```

| Field                  | Type     | Required | Description                  |
| :--------------------- | :------- | :------: | :--------------------------- |
| `projectId`            | string   |    ✓     | Project ID                   |
| `approvedCandidateIds` | string[] |    ✓     | IDs of candidates to approve |

### qa_use_case_list

List graduated use cases for a project.

### qa_use_case_get

Get details of a specific use case.

---

## Workflow Tools

Workflows are long-running operations. After starting, use `get_latest_run` tools to check status.

```mermaid
flowchart LR
    start["Start Workflow"] --> poll["Poll Status"]
    poll --> check{"Status?"}
    check -->|queued/running| poll
    check -->|succeeded| done["Get Results"]
    check -->|failed| error["Handle Error"]
```

### Website Scan

#### qa_workflow_start_website_scan

Start scanning a website to discover use cases.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "url": "https://example.com",
  "description": "Discover e-commerce user flows",
  "archiveUnapproved": true
}
```

| Field               | Type    | Required | Description                                  |
| :------------------ | :------ | :------: | :------------------------------------------- |
| `projectId`         | string  |    ✓     | Target project                               |
| `url`               | string  |    ✓     | Website URL to scan                          |
| `description`       | string  |    ✓     | What to look for                             |
| `archiveUnapproved` | boolean |          | Archive existing unapproved candidates first |

**Output:**

```json
{
  "workflowRuntimeId": "wfrt_scan_123"
}
```

#### qa_workflow_get_website_scan_latest_run

Check scan status.

**Output:**

```json
{
  "workflowRunId": "wfrun_456",
  "status": "succeeded",
  "updatedAt": "2024-01-15T10:35:00Z"
}
```

**Workflow statuses:**

| Status      | Description            |
| :---------- | :--------------------- |
| `queued`    | Waiting to start       |
| `running`   | In progress            |
| `succeeded` | Completed successfully |
| `failed`    | Completed with errors  |
| `cancelled` | Stopped by user        |

#### qa_workflow_list_website_scan_runtimes

List all website scan runtimes.

### Test Case Detection

#### qa_workflow_start_test_case_detection

Generate test cases from approved use cases.

| Field       | Type   | Required | Description                      |
| :---------- | :----- | :------: | :------------------------------- |
| `projectId` | string |    ✓     | Target project                   |
| `useCaseId` | string |          | Specific use case (omit for all) |

#### qa_workflow_get_test_case_detection_latest_run

Check test case detection status.

#### qa_workflow_list_test_case_detection_runtimes

List test case detection runtimes.

### Test Script Generation

#### qa_workflow_start_test_script_generation

Generate executable test scripts from test cases.

| Field        | Type   | Required | Description        |
| :----------- | :----- | :------: | :----------------- |
| `projectId`  | string |    ✓     | Target project     |
| `testCaseId` | string |          | Specific test case |
| `useCaseId`  | string |          | Specific use case  |

#### qa_workflow_get_test_script_generation_latest_run

Check script generation status.

#### qa_workflow_get_latest_test_script_generation_runtime_by_test_case

Get the latest generation runtime for a specific test case.

### Test Script Replay

#### qa_workflow_start_test_script_replay

Execute a single test script.

| Field          | Type   | Required | Description       |
| :------------- | :----- | :------: | :---------------- |
| `projectId`    | string |    ✓     | Target project    |
| `testScriptId` | string |    ✓     | Script to execute |

#### qa_workflow_start_test_script_replay_bulk

Execute multiple test scripts in parallel.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "testScriptIds": ["ts_301", "ts_302"],
  "name": "Nightly regression"
}
```

| Field           | Type     | Required | Description                     |
| :-------------- | :------- | :------: | :------------------------------ |
| `projectId`     | string   |    ✓     | Target project                  |
| `testScriptIds` | string[] |          | Specific scripts (omit for all) |
| `name`          | string   |          | Name for this run batch         |

#### qa_workflow_get_test_script_replay_bulk_latest_run

Check bulk replay status.

#### qa_workflow_get_replay_bulk_run_batch_summary

Get detailed summary of a bulk run batch.

**Output:**

```json
{
  "runBatchId": "batch_789",
  "totalScripts": 10,
  "passed": 8,
  "failed": 2
}
```

### Workflow Management

#### qa_workflow_cancel_run

Cancel a specific workflow run.

#### qa_workflow_cancel_runtime

Cancel a workflow runtime and all its runs.

---

## Artifact Tools

### Test Cases

#### qa_test_case_list

List test cases for a project.

#### qa_test_case_get

Get test case details including steps and expected outcomes.

#### qa_test_case_list_by_use_case

List test cases for a specific use case.

### Test Scripts

#### qa_test_script_list

List test scripts for a project.

#### qa_test_script_get

Get test script details including executable steps.

#### qa_test_script_list_paginated

List test scripts with full pagination.

### Summaries

#### qa_project_test_results_summary_get

Get aggregated test results summary.

**Output:**

```json
{
  "projectId": "proj_abc123",
  "totalTests": 25,
  "passed": 22,
  "failed": 3,
  "passRate": 88.0
}
```

#### qa_project_test_scripts_summary_get

Get test scripts summary.

#### qa_project_test_runs_summary_get

Get recent test runs summary.

---

## Report Tools

### qa_report_stats_summary_get

Get report statistics and health score.

### qa_report_cost_query

Query usage and cost data.

| Field          | Type   | Required | Description           |
| :------------- | :----- | :------: | :-------------------- |
| `projectId`    | string |    ✓     | Target project        |
| `startDateKey` | string |          | Start date (YYYYMMDD) |
| `endDateKey`   | string |          | End date (YYYYMMDD)   |

### qa_report_preferences_upsert

Configure report delivery preferences.

**Input:**

```json
{
  "projectId": "proj_abc123",
  "channels": ["email", "webhook"],
  "emails": ["team@example.com"],
  "webhookUrl": "https://hooks.example.com/muggle"
}
```

| Field        | Type     | Required | Description                            |
| :----------- | :------- | :------: | :------------------------------------- |
| `projectId`  | string   |    ✓     | Target project                         |
| `channels`   | string[] |    ✓     | Delivery channels                      |
| `emails`     | string[] |          | Email addresses (for email channel)    |
| `phones`     | string[] |          | Phone numbers (for SMS channel)        |
| `webhookUrl` | string   |          | Webhook endpoint (for webhook channel) |

**Available channels:**

| Channel   | Description                     |
| :-------- | :------------------------------ |
| `email`   | Send reports as PDF attachments |
| `sms`     | Send summary notifications      |
| `webhook` | POST JSON payload to endpoint   |

### qa_report_final_generate

Generate a final test report (async operation).

---

## Recommendation Tools

These tools provide guidance without making changes.

### qa_recommend_schedule

Get test scheduling recommendations.

**Output:**

```json
{
  "recommendations": [
    {
      "title": "Nightly Regression Tests",
      "rationale": "Running tests every night catches regressions quickly",
      "schedule": "0 2 * * *",
      "tradeoffs": ["Pro: Fresh results every morning", "Con: Delayed feedback"]
    }
  ]
}
```

### qa_recommend_cicd_setup

Get CI/CD integration templates for GitHub Actions, Azure DevOps, GitLab CI, etc.

| Field                | Type   | Options                                    |
| :------------------- | :----- | :----------------------------------------- |
| `repositoryProvider` | string | `github`, `azureDevOps`, `gitlab`, `other` |
| `cadence`            | string | `onPullRequest`, `nightly`, `onDemand`     |

---

## Error Responses

All tools may return errors:

```json
{
  "error": "ERROR_CODE",
  "message": "Human-readable description",
  "details": {}
}
```

| Error Code         | HTTP Status | Description                       |
| :----------------- | :---------: | :-------------------------------- |
| `UNAUTHORIZED`     |     401     | Missing or invalid authentication |
| `FORBIDDEN`        |     403     | Access denied to resource         |
| `NOT_FOUND`        |     404     | Resource doesn't exist            |
| `INVALID_ARGUMENT` |     400     | Invalid input parameters          |
| `UPSTREAM_ERROR`   |     502     | Backend service error             |
| `INTERNAL_ERROR`   |     500     | Unexpected server error           |

---

## Next Steps

- **[MCP Quickstart](getting-started/mcp-quickstart.md)** - Get started
- **[MCP Concepts](mcp/mcp-concepts.md)** - Understand the architecture
- **[CI/CD Integration](mcp/mcp-cicd-integration.md)** - Automate in pipelines
