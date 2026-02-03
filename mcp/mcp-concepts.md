# MCP Concepts & Architecture

Understanding how the MCP Gateway works with Muggle Test.

## System Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Your Environment                             │
│  ┌──────────────┐                                                   │
│  │  AI Assistant │  (Claude, Cursor, Custom MCP Client)             │
│  │  ┌─────────┐ │                                                   │
│  │  │   MCP   │ │                                                   │
│  │  │ Client  │ │                                                   │
│  └──┴────┬────┴─┘                                                   │
└──────────┼──────────────────────────────────────────────────────────┘
           │ HTTPS (MCP Streamable HTTP)
           │ + API Key Authentication
           ▼
┌─────────────────────────────────────────────────────────────────────┐
│                      Muggle AI Cloud                                 │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    MCP QA Gateway                             │   │
│  │  • Protocol translation (MCP ↔ REST)                         │   │
│  │  • Authentication forwarding                                  │   │
│  │  • Rate limiting & security                                   │   │
│  └──────────────────────────┬───────────────────────────────────┘   │
│                             │                                        │
│  ┌──────────────────────────▼───────────────────────────────────┐   │
│  │                    Muggle Test Platform                       │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │   │
│  │  │   Project   │  │   Workflow  │  │   Report    │          │   │
│  │  │  Management │  │   Engine    │  │  Generator  │          │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘          │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                             │                                        │
│  ┌──────────────────────────▼───────────────────────────────────┐   │
│  │                    Test Execution                             │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐          │   │
│  │  │   Browser   │  │     AI      │  │  Screenshot │          │   │
│  │  │  Automation │  │   Analysis  │  │   Capture   │          │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘          │   │
│  └──────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

## What is MCP?

The [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) is an open standard for connecting AI assistants to external tools and data sources. It provides:

- **Standardized Interface**: Common protocol for tool invocation across different AI assistants
- **Streaming Support**: Real-time updates for long-running operations
- **Tool Discovery**: AI assistants can automatically discover available capabilities

## Core Concepts

### Projects

A **Project** is the top-level container for all your testing artifacts. Each project typically corresponds to one website or application.

```
Project
├── Use Cases (user flows to test)
│   ├── Test Cases (specific scenarios)
│   │   └── Test Scripts (executable automation)
├── Secrets (stored credentials)
├── PRD Files (requirements documents)
└── Reports (test results)
```

### Use Cases

A **Use Case** represents a user flow or feature to test (e.g., "User Login", "Checkout Process").

**Lifecycle:**

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Website   │────▶│  Candidate  │────▶│  Graduated  │
│    Scan     │     │   (Draft)   │     │ (Approved)  │
└─────────────┘     └─────────────┘     └─────────────┘
                          │
                          ▼
                    ┌─────────────┐
                    │   Dropped   │
                    │ (Rejected)  │
                    └─────────────┘
```

1. **Discovery**: Website scan identifies potential use cases
2. **Candidate**: Proposed use cases await your review
3. **Graduated**: Approved use cases become active test targets
4. **Dropped**: Rejected candidates are archived

### Test Cases

A **Test Case** is a specific scenario derived from a use case. Each use case may have multiple test cases:

- Use Case: "User Login"
  - Test Case 1: Valid credentials → successful login
  - Test Case 2: Invalid password → error message
  - Test Case 3: Empty fields → validation errors

### Test Scripts

A **Test Script** is executable automation generated from a test case. Scripts contain:
- Step-by-step browser actions
- Assertions and validations
- Screenshots at each step

### Workflows

**Workflows** are long-running operations:

| Workflow | Purpose | Output |
|----------|---------|--------|
| Website Scan | Discover use cases | Use case candidates |
| Test Case Detection | Generate test cases | Test cases |
| Test Script Generation | Create automation | Test scripts |
| Test Script Replay | Execute tests | Test results |
| Report Generation | Create reports | PDF/HTML reports |

**Workflow states:**
- `queued` - Waiting to start
- `running` - In progress
- `succeeded` - Completed successfully
- `failed` - Completed with errors
- `cancelled` - Stopped by user

## Testing Flow

```
                    ┌─────────────────────────────┐
                    │     1. CREATE PROJECT       │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │     2. WEBSITE SCAN         │
                    │   Discover user flows       │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │  3. REVIEW & APPROVE        │
                    │   Select use cases          │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │  4. GENERATE TEST CASES     │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │  5. GENERATE SCRIPTS        │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │     6. RUN TESTS            │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │   7. VIEW RESULTS           │
                    └─────────────┬───────────────┘
                                  │
                    ┌─────────────▼───────────────┐
                    │   8. GENERATE REPORT        │
                    └─────────────────────────────┘
```

## Authentication

### API Keys

API keys provide programmatic access:

```
mai_sk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
└─┬──┘ └─┬──────────────────────────────┘
  │      │
  │      └── Secret portion (keep private)
  └── Prefix identifying key type
```

**Best practices:**
- Rotate keys periodically
- Use separate keys for different environments
- Never expose keys in client-side code
- Store keys in environment variables or secrets managers

### Authorization

All API calls are scoped to your account:
- You can only access projects you own or have been granted access to
- Team members can be granted specific roles
- API keys inherit the permissions of the creating user

## Rate Limits

The gateway enforces rate limits to ensure fair usage:

| Tier | Requests/minute | Concurrent workflows |
|------|-----------------|---------------------|
| Free | 20 | 1 |
| Pro | 60 | 5 |
| Enterprise | 300 | 20 |

Rate limit headers are included in responses:
- `X-RateLimit-Limit`: Maximum requests allowed
- `X-RateLimit-Remaining`: Requests remaining
- `X-RateLimit-Reset`: Time when limit resets

## Available Tools

The gateway provides 46+ tools organized into categories:

| Category | Tools | Purpose |
|----------|-------|---------|
| Project | 4 | Create and manage projects |
| PRD Files | 3 | Upload requirements documents |
| Secrets | 5 | Manage test credentials |
| Use Cases | 4 | Discover and approve use cases |
| Workflows | 17 | Execute testing workflows |
| Artifacts | 9 | Inspect test cases and scripts |
| Reports | 4 | Generate and deliver reports |
| Recommendations | 2 | Get scheduling guidance |

## Next Steps

- **[MCP Quickstart](../getting-started/mcp-quickstart.md)** - Get started in 5 minutes
- **[MCP API Reference](./mcp-api-reference.md)** - Complete tool documentation
- **[CI/CD Integration](./mcp-cicd-integration.md)** - Automate in your pipeline
