# Local Testing Overview

Test your localhost applications directly from your AI assistant using Muggle Test.

## On This Page

| Section | Description |
| :------ | :---------- |
| [What is Local Testing?](#what-is-local-testing) | Overview and supported environments |
| [Why Local Testing?](#why-local-testing) | Challenges it solves |
| [Key Features](#key-features) | Localhost access, AI-driven testing, screenshots, and more |
| [The Four-Step Flow](#the-four-step-flow) | From description to results |
| [Comparison: Local vs Cloud Testing](#comparison-local-vs-cloud-testing) | Feature comparison table |
| [Quick Example](#quick-example) | Sample conversation |
| [Cloud-First Local Testing Workflow](#cloud-first-local-testing-workflow) | Architecture and workflow steps |
| [Getting Started](#getting-started) | Links to setup and guides |
| [Requirements](#requirements) | Node.js, MCP client, and OS |

## What is Local Testing?

Local Testing is a capability of the `@muggleai/works` package that enables AI coding agents (Claude Code, Cursor, Antigravity) to run real-browser end-to-end (E2E) acceptance tests against web applications on your development machine. A real Electron browser clicks buttons, fills forms, navigates pages, and captures screenshots to verify user flows work correctly. Unlike cloud E2E testing which tests publicly accessible URLs, local testing runs entirely on your computer and can access `localhost` URLs.

## When to Use Local vs Cloud Testing

| Environment | Example | Which to Use |
| :---------- | :------ | :----------- |
| Local development | `localhost:3000` | **Local Testing** |
| Docker containers | `localhost:8080` | **Local Testing** |
| Local network | `192.168.1.x` | **Local Testing** |
| Preview deployments | `pr-123.preview.example.com` | Cloud E2E |
| Staging | `staging.example.com` | Cloud E2E |
| Production | `www.example.com` | Cloud E2E |

```mermaid
flowchart LR
    subgraph dev["Your Computer"]
        A["AI Assistant<br/>(Cursor, Claude)"]
        B["@muggleai/works"]
        C["Browser Engine"]
        D["Your App<br/>(localhost:3000)"]
        
        A -->|"MCP Protocol"| B
        B -->|"Browser Automation"| C
        C -->|"HTTP"| D
    end
```

## Why Local Testing?

| Challenge | Solution |
| :-------- | :------- |
| Cloud services can't access localhost | Runs entirely on your machine |
| Context switching between IDE and browser | Test directly from your coding assistant |
| Manual testing slows development | AI-driven test automation |
| Hard to describe bugs to teammates | Screenshots and test results captured automatically |

## Key Features

| Feature | Description |
| :------ | :---------- |
| **Localhost Access** | Test `localhost`, `127.0.0.1`, or any local dev server |
| **AI-Driven Testing** | Describe tests in natural language |
| **Cloud-First Architecture** | Manage entities in cloud (`muggle-remote-*`), execute locally (`muggle-local-*`) |
| **Test Script Generation** | AI generates repeatable test scripts from test cases |
| **Browser Automation** | Real browser interactions (click, type, scroll) |
| **Screenshot Capture** | Visual documentation of test results |
| **Agent Skills** | Pre-built workflows like "test my changes" |
| **Publish Test to Cloud** | Upload locally generated test scripts to Muggle Test for replay and collaboration |

## The Four-Step Flow

```mermaid
flowchart TD
    subgraph step1["1. You Describe the Test"]
        A["'Test the login flow<br/>on localhost:3000'"]
    end
    
    subgraph step2["2. AI Interprets Intent"]
        B["AI assistant understands<br/>what to test and how"]
    end
    
    subgraph step3["3. Browser Executes"]
        C["Real browser navigates,<br/>clicks, types, validates"]
    end
    
    subgraph step4["4. Results Returned"]
        D["Screenshots, logs, and<br/>pass/fail status"]
    end
    
    step1 --> step2 --> step3 --> step4
```

1. **You describe what to test** in natural language to your AI assistant
2. **The assistant translates** your request into browser automation commands
3. **A real browser** (Electron/Chromium) executes the actions on your localhost app
4. **Results come back** with screenshots, logs, and status—all in your IDE

## Comparison: Local vs Cloud Testing

| Aspect | Local Testing | Cloud E2E |
| :----- | :------------ | :------- |
| **Target URLs** | `localhost`, local network | Public URLs (preview, staging, prod) |
| **Setup** | `muggle serve --local` | `muggle serve --e2e` |
| **Browser runs on** | Your machine | Muggle AI cloud |
| **Test results** | Local files (`~/.muggle-ai/`) | Cloud dashboard |
| **Best for** | Development, debugging | CI/CD, production testing |
| **Subscription** | Free (local only) | Requires plan |

## When to Use Each

| Scenario | Recommendation |
| :------- | :------------- |
| Testing during local development | **Local Testing** |
| Debugging a specific bug locally | **Local Testing** |
| Testing before committing code | **Local Testing** |
| Testing preview/PR deployments | Cloud E2E |
| Automated CI/CD testing | Cloud E2E |
| Testing staging or production | Cloud E2E |
| Team collaboration on test results | Cloud E2E |

## Quick Example

Here's what local testing looks like in practice:

---

**You**: Test the signup form on my local app at localhost:3000/signup. Try submitting with an invalid email and verify the error message appears.

**Assistant**: I'll test the signup form validation on your local app.

*[Runs test against localhost:3000/signup]*

Test completed:
- Navigated to localhost:3000/signup
- Entered "invalid-email" in the email field
- Clicked Submit
- **Result**: Error message "Please enter a valid email" appeared ✓

Screenshot saved to `~/.muggle-ai/sessions/session-xxx/screenshots/step-003.png`

---

## Cloud-First Local Testing Workflow

Local Testing uses a **cloud-first architecture**:

- **`muggle-remote-*` tools**: All entity management (projects, use cases, test cases) happens in the cloud
- **`muggle-local-*` tools**: Local execution, results viewing, and publishing only

```mermaid
flowchart LR
    subgraph cloud["Cloud (muggle-remote-* tools)"]
        A["Create Project"]
        B["Define Use Cases"]
        C["Create Test Cases"]
    end
    
    subgraph local["Local (muggle-local-* tools)"]
        D["Execute Tests<br/>(against localhost)"]
        E["View Results"]
        F["Publish Scripts"]
    end
    
    subgraph production["Production"]
        G["Team Collaboration"]
        H["Scheduled Runs"]
        I["CI/CD Integration"]
    end
    
    A --> B --> C --> D --> E --> F --> G & H & I
```

1. **Create in cloud** — Use `muggle-remote-*` tools to create projects, use cases, and test cases
2. **Fetch details** — Call `muggle-remote-test-case-get` to retrieve test case information
3. **Execute locally** — Use `muggle-local-execute-test-generation` with localhost URL
4. **View results** — Use `muggle-local-run-result-get` to see screenshots and status
5. **Publish** — Use `muggle-local-publish-test-script` to upload generated scripts (returns a `viewUrl` to open the dashboard)
6. **Scale** — Cloud enables team collaboration and CI/CD integration

## Getting Started

Ready to test your localhost applications?

1. **[Local Testing Setup](setup.md)** — Install and configure in 10 minutes
2. **[Agent Skills](skills.md)** — Pre-built workflows for "test my changes" and quick single-feature tests
3. **[Available Tools](tools-reference.md)** — Complete tool documentation
4. **[Example Workflows](examples.md)** — Common testing scenarios

## Requirements

| Requirement | Version |
| :---------- | :------ |
| Node.js | 22 or later |
| MCP Client | Claude Code, Cursor, Antigravity, or any MCP-compatible client |
| Operating System | macOS, Windows, or Linux |
