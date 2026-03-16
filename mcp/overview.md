# MCP Overview

The Muggle AI MCP package lets you automate testing through AI assistants using the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/).

## What is the MCP Package?

The `@muggleai/mcp` package is a **unified MCP server** that provides:

| Feature | Description |
| :------ | :---------- |
| **Local Testing** | Test localhost and local network applications |
| **Cloud QA** | Test preview, staging, and production environments |
| **AI-Driven Automation** | Use natural language to control testing workflows |
| **Single Installation** | One package for all testing needs |

## Quick Install

```bash
npm install -g @muggleai/mcp@latest
```

## When to Use Each Mode

| Environment | Example URLs | Mode |
| :---------- | :----------- | :--- |
| Local Dev | `localhost:3000` | Local (`--local`) |
| Docker | `localhost:8080` | Local (`--local`) |
| Local Network | `192.168.1.x` | Local (`--local`) |
| Preview | `pr-123.preview.example.com` | Cloud (`--qa`) |
| Staging | `staging.example.com` | Cloud (`--qa`) |
| Production | `www.example.com` | Cloud (`--qa`) |
| Both | Any combination | Default (no flags) |

## Architecture

### Local Testing Mode

```mermaid
flowchart LR
    subgraph dev["Your Computer"]
        A["AI Assistant<br/>(Cursor, Claude)"]
        B["@muggleai/mcp"]
        C["Browser Engine"]
        D["Your App<br/>(localhost:3000)"]
        
        A -->|"MCP Protocol"| B
        B -->|"Automation"| C
        C -->|"HTTP"| D
    end
```

Local testing runs entirely on your machine:
- Browser automation engine executes tests
- Results stored locally in `~/.muggle-ai/`
- No cloud connection required

### Cloud QA Mode

```mermaid
flowchart LR
    subgraph env["Your Computer"]
        A["AI Assistant"]
        B["@muggleai/mcp"]
    end
    
    subgraph cloud["Muggle AI Cloud"]
        C[Test Platform]
        D[Browser Farm]
    end
    
    subgraph target["Target"]
        E["Your App<br/>(staging, production)"]
    end
    
    A -->|"MCP"| B
    B -->|"HTTPS + API Key"| C
    C -->|"Test Execution"| D
    D -->|"HTTP"| E
```

Cloud QA uses Muggle AI's infrastructure:
- Tests run on cloud browser farm
- Results stored in cloud dashboard
- Supports CI/CD integration

### Unified Mode (Default)

When you run `muggle-mcp serve` without flags, **both modes are active**:
- Use `muggle-local-*` tools for local testing
- Use `muggle-remote-*` tools for cloud QA
- Seamlessly publish local projects to cloud

## Key Benefits

### Single Installation

No need to manage multiple packages. The unified `@muggleai/mcp` package replaces all previous packages:

| Previous (Deprecated) | Current |
| :-------------------- | :------ |
| `@muggleai/mcp-qa-gateway` | `@muggleai/mcp` |
| `@muggle-ai/local-mcp` | `@muggleai/mcp` |
| Two configurations | One configuration |

**Note:** The previous packages have been sunset and are no longer maintained. Use `@muggleai/mcp` for all new installations.

### Natural Language Testing

Instead of navigating dashboards or writing scripts:

> "Create a project for my e-commerce site and test the checkout flow"

### Seamless Local-to-Cloud Workflow

1. **Develop locally** - Test on localhost
2. **Generate scripts** - AI creates automation
3. **Publish to cloud** - One command to deploy
4. **Scale testing** - Run in CI/CD

## Tool Categories

| Category | Count | Description |
| :------- | ----: | :---------- |
| **Local Testing** | ~65 | Project management, execution, results |
| **Cloud QA** | ~60 | Workflows, test generation, reporting |
| **Total** | ~125 | All testing needs covered |

### Local Tools (prefix: `muggle-local-`)

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| Authentication | `muggle-local-auth-*` | Login, logout, status |
| Projects | `muggle-local-project-*` | Create and manage projects |
| Use Cases | `muggle-local-use-case-*` | Define user flows |
| Test Cases | `muggle-local-test-case-*` | Test specifications |
| Test Scripts | `muggle-local-test-script-*` | Automation scripts |
| Execution | `muggle-local-execute-*` | Run tests |
| Results | `muggle-local-run-result-*` | View results |
| Cloud Sync | `muggle-local-cloud-*`, `muggle-local-publish-*` | Sync with cloud |

### Cloud QA Tools (prefix: `muggle-remote-`)

| Category | Tools | Purpose |
| :------- | :---- | :------ |
| Projects | `muggle-remote-project-*` | Cloud project management |
| Use Cases | `muggle-remote-use-case-*` | Use case discovery |
| Test Cases | `muggle-remote-test-case-*` | Test case generation |
| Workflows | `muggle-remote-workflow-*` | Automated workflows |
| Reports | `muggle-remote-report-*` | Test reporting |
| Secrets | `muggle-remote-secret-*` | Credential management |

## Getting Started

Ready to connect your AI assistant to Muggle Test?

1. **[MCP Quickstart](../getting-started/mcp-quickstart.md)** - Get started in 5 minutes
2. **[MCP Installation](mcp-installation.md)** - Detailed setup options
3. **[Local Testing Setup](../local-testing/setup.md)** - Focus on localhost testing
4. **[MCP Concepts](mcp-concepts.md)** - Understand the architecture
5. **[MCP API Reference](mcp-api-reference.md)** - Complete tool documentation

## Requirements

| Requirement | Version |
| :---------- | :------ |
| Node.js | 22+ |
| MCP Client | Cursor, Claude Desktop, or compatible |
| Operating System | macOS, Windows, or Linux |
