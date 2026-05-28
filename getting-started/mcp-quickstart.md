# MCP Quickstart

Connect your AI assistant to Muggle Test in under 5 minutes.

## On This Page

| Section | Description |
| :------ | :---------- |
| [Choose Your Platform](#choose-your-platform) | Install and configure for Claude Code, Cursor, or Antigravity |
| [Verify Installation](#verify-installation) | Confirm everything is working |
| [Authentication](#authentication-optional-for-local-testing) | Set up cloud access (optional for local testing) |
| [Run Your First Test](#run-your-first-test) | Local and cloud testing walkthroughs |
| [Example Conversation](#example-conversation) | End-to-end session example |
| [Tool Overview](#tool-overview) | Available tool categories |
| [Choosing Local vs Cloud](#choosing-local-vs-cloud) | When to use each mode |
| [Hosted Gateway](#hosted-gateway-alternative) | Cloud-only setup without local install |
| [Troubleshooting](#troubleshooting) | Common issues and fixes |
| [Next Steps](#next-steps) | Further reading |

## Prerequisites

| Requirement | Description |
| :---------- | :---------- |
| Node.js | Version 22 or later |
| MCP client | Claude Code, Cursor, or Antigravity |

---

## Choose Your Platform

### Claude Code

Install via the plugin system — no manual config files needed:

```
/plugin marketplace add https://github.com/multiplex-ai/muggle-ai-works
/plugin install muggleai@muggle-works
```

This delivers skills, MCP server, and hooks through the plugin lifecycle.

### Cursor

**1. Install the package globally:**

```bash
npm install -g @muggleai/works@latest
```

For Cursor, that's it — the install automatically configures `~/.cursor/mcp.json`. Just restart Cursor.

For other MCP clients, add this to your client's config:

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"]
    }
  }
}
```

**2. Restart Cursor** to pick up the new configuration.

### Antigravity

**1. Install the package globally:**

```bash
npm install -g @muggleai/works@latest
```

**2. Configure MCP** in `~/.gemini/antigravity/mcp_config.json`:

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"]
    }
  }
}
```

You can also add it via the Antigravity UI: open the **Agent panel** → **MCP Servers** → **Manage MCP Servers** → **View raw config**.

**3. Restart Antigravity** to pick up the new configuration.

---

## Verify Installation

| Platform | How to verify |
| :------- | :------------ |
| **Claude Code** | Run `/muggle:status` |
| **Cursor** | Ask your assistant: **"Check the Muggle Test status"** |
| **Antigravity** | Ask your agent: **"Check the Muggle Test status"** |

---

## Authentication (Optional for Local Testing)

For **local testing only**, no authentication is needed. For **cloud features**, authenticate:

### Option A: Agentic Authentication (Recommended)

Ask your assistant: **"Help me log in to Muggle Test"**

The assistant will:
1. Generate a login URL and code
2. Wait for you to complete browser login
3. Store credentials locally

### Option B: API Key

1. Log in to [Muggle Test Dashboard](https://www.muggle-ai.com)
2. Navigate to **Settings** → **API Keys**
3. Create and copy your API key
4. Add the key to your MCP configuration:

**Claude Code:** API key is stored via the agentic auth flow — no config change needed.

**Cursor** (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

**Antigravity** (`~/.gemini/antigravity/mcp_config.json`):

```json
{
  "mcpServers": {
    "muggle": {
      "command": "muggle",
      "args": ["serve"],
      "env": {
        "MCP_API_KEY": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

---

## Run Your First Test

### Local Testing (localhost)

```mermaid
flowchart LR
    A["Create Project"] --> B["Add Test Case"]
    B --> C["Generate Script"]
    C --> D["Run Test"]
    D --> E["View Results"]
```

**1. Create a project:**

> "Create a test project for my app at http://localhost:3000"

**2. Add a test case:**

> "Create a test case to verify the login form shows an error for invalid email"

**3. Generate a test script:**

> "Generate a test script for the login test case"

**4. Run the test:**

> "Run the login test"

### Cloud E2E acceptance testing (remote URLs)

For testing staging, preview, or production URLs:

**1. Create a project:**

> "Create a project for E2E acceptance tests called 'My Website Tests' for testing https://example.com"

**2. Discover use cases:**

> "Scan the website and discover potential test use cases"

**3. Review and approve:**

> "Show me the discovered use case candidates"
> "Approve the login and checkout use cases"

**4. Generate and run tests:**

> "Generate test cases for the approved use cases, then run all tests"

**5. View results:**

> "Show me the test results summary"

---

## Example Conversation

**You**: I want to test my app running at localhost:3000

**Assistant**: I'll help you set up testing. Let me create a project first.

*[Creates project]*

Done! I've created a project called "localhost:3000 Tests". What would you like to test?

---

**You**: Test that the signup form validates email format

**Assistant**: I'll create a test case for email validation.

*[Creates test case]*

Created! Now I'll generate a test script by automating the browser.

*[Launches browser, performs test steps, captures screenshots]*

Test script generated successfully! The test navigated to the signup page, entered an invalid email, and verified the error message appeared.

Would you like me to run the test now?

---

**You**: Yes, run it

**Assistant**: Running the test...

*[Executes test script]*

**Result: PASSED** (2.3s)

The signup form correctly shows "Please enter a valid email" when an invalid email is submitted.

---

## Tool Overview

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| **Cloud entities** | `muggle-remote-project-*`, `muggle-remote-use-case-*`, `muggle-remote-test-case-*` | Create and manage projects, use cases, and test cases (used by both local and cloud testing) |
| **Cloud workflows** | `muggle-remote-workflow-*`, `muggle-remote-report-*` | Cloud test generation, replay, and reporting |
| **Local execution** | `muggle-local-execute-*`, `muggle-local-run-result-*` | Generate and replay scripts locally, view results |
| **Publishing** | `muggle-local-publish-test-script` | Publish a locally generated script to the cloud |

**Total: 100+ tools** covering both local and cloud testing.

---

## Choosing Local vs Cloud

| Scenario | Recommendation |
| :------- | :------------- |
| Testing during development | **Local** (`muggle serve --local`) |
| Testing localhost URLs | **Local** |
| Testing preview/staging deployments | **Cloud** (`muggle serve --e2e`) |
| CI/CD integration | **Cloud** |
| Team collaboration | **Cloud** |
| Both local and cloud | **Default** (`muggle serve`) |

---

## Hosted Gateway (Alternative)

For cloud-only testing without local installation, use the hosted gateway. This works with any MCP-compatible client:

**Cursor** (`~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "muggle": {
      "url": "https://mcp.muggle-ai.com/mcp",
      "headers": {
        "x-api-key": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

**Antigravity** (`~/.gemini/antigravity/mcp_config.json`):

```json
{
  "mcpServers": {
    "muggle": {
      "url": "https://mcp.muggle-ai.com/mcp",
      "headers": {
        "x-api-key": "mai_sk_your_api_key_here"
      }
    }
  }
}
```

---

## Troubleshooting

### Connection Issues

| Check | Solution |
| :---- | :------- |
| Package installed? | `npm list -g @muggleai/works` |
| Config correct? | Verify JSON syntax in your platform's config file |
| Client restarted? | Restart after config changes |
| Run doctor? | `muggle doctor` |

### Authentication Errors

| Check | Solution |
| :---- | :------- |
| API key correct? | Verify key in dashboard |
| Key expired? | Regenerate in dashboard |
| Key format? | Should start with `mai_sk_` |

### Platform-Specific Config Paths

| Platform | Config file location |
| :------- | :------------------- |
| Claude Code | Managed via plugin system — no manual config |
| Cursor | `~/.cursor/mcp.json` |
| Antigravity | `~/.gemini/antigravity/mcp_config.json` |

---

## Next Steps

- **[MCP Installation](mcp/mcp-installation.md)** - Detailed setup options
- **[Local Testing Setup](local-testing/setup.md)** - In-depth local testing guide
- **[MCP Concepts](mcp/mcp-concepts.md)** - Understand the architecture
- **[MCP API Reference](mcp/mcp-api-reference.md)** - Complete tool documentation
- **[CI/CD Integration](mcp/mcp-cicd-integration.md)** - Automate in your pipeline
