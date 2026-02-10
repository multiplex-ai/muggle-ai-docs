# MCP Gateway Overview

The MCP QA Gateway lets you automate Muggle Test through AI assistants using the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/).

## What is the MCP Gateway?

The MCP Gateway exposes Muggle Test capabilities through MCP, enabling:

- **AI Assistant Integration**: Control testing workflows through Claude, Cursor, or any MCP-compatible assistant
- **Natural Language Testing**: Use conversational commands instead of clicking through UIs
- **Programmable Automation**: Build custom integrations with MCP clients

## Deployment Options

| Option | Description | Setup |
| :----- | :---------- | :---- |
| **Hosted** | Use Muggle AI's hosted gateway | Just configure your MCP client |
| **Local** | Run gateway on your machine | Install npm package |

### Hosted Mode

```mermaid
flowchart LR
    subgraph env["Your Environment"]
        A["AI Assistant<br/>(Claude, Cursor, etc.)"]
    end
    
    subgraph cloud["Muggle AI Cloud"]
        B[MCP Gateway]
        C[Muggle Test Platform]
    end
    
    A -->|"HTTPS + API Key"| B
    B -->|REST API| C
```

Your AI assistant connects to the hosted gateway via HTTPS. No installation required.

### Local Mode

```mermaid
flowchart LR
    subgraph env["Your Computer"]
        A2["AI Assistant"]
        B2["Local Gateway<br/>(npm package)"]
        A2 -->|"stdin/stdout"| B2
    end
    
    subgraph cloud2["Muggle AI Cloud"]
        C2[Muggle Test Platform]
    end
    
    B2 -->|"HTTPS + API Key"| C2
```

Install the gateway locally for lower latency and offline development:

```bash
npm install -g @muggleai/mcp-qa-gateway
```

See [MCP Installation](mcp-installation.md) for detailed setup instructions.

## Key Benefits

### Conversational Testing

Instead of navigating dashboards, simply tell your AI assistant what you want:

> "Create a project for my e-commerce site and discover testable user flows"

### Seamless Integration

Works with any MCP-compatible client:

| Client             | Description                    |
| :----------------- | :----------------------------- |
| **Claude Desktop** | Anthropic's AI assistant       |
| **Cursor**         | AI-powered code editor         |
| **Custom clients** | Build your own MCP integration |

### Full Platform Access

The gateway exposes 50+ tools covering the complete Muggle Test workflow:

| Capability         | Description                                   |
| :----------------- | :-------------------------------------------- |
| Project management | Create, configure, and organize test projects |
| Use case discovery | Automatically find testable user flows        |
| Test generation    | Generate test cases and automation scripts    |
| Test execution     | Run tests and collect results                 |
| Reporting          | Generate and deliver test reports             |

## Getting Started

Ready to connect your AI assistant to Muggle Test?

1. **[MCP Quickstart](../getting-started/mcp-quickstart.md)** - Connect in 5 minutes
2. **[MCP Installation](mcp-installation.md)** - Local vs hosted setup options
3. **[MCP Concepts](mcp-concepts.md)** - Understand the architecture
4. **[MCP API Reference](mcp-api-reference.md)** - Complete tool documentation

## Prerequisites

- A Muggle Test account with an active subscription
- An API key from your dashboard
- An MCP-compatible client (Claude Desktop, Cursor, etc.)
