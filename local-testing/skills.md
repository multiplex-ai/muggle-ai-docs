# Agent Skills for Local Testing

Pre-built AI agent skills that automate common testing workflows with Muggle AI.

## What are Agent Skills?

Agent Skills are structured instructions that teach AI assistants how to perform complex, multi-step workflows. Instead of manually calling individual tools, you can simply say "test my changes" and the agent follows the skill's workflow automatically.

## Local vs Remote Skills

The [muggle-ai-mcp repository](https://github.com/muggle-ai/muggle-ai-mcp) organizes skills by testing mode:

| Folder | MCP Package Mode | Use For |
| :----- | :--------------- | :------ |
| `skills/local/` | `muggle-mcp serve --local` | `localhost`, local network |
| `skills/remote/` | `muggle-mcp serve --qa` | Public URLs (staging, prod) |

Both modes are available with the unified `@muggleai/mcp` package. **This page covers local skills.** For remote testing, see [Remote Testing with MCP](../mcp/overview.md).

## Available Local Skills

| Skill | Description | Triggers |
| :---- | :---------- | :------- |
| **test-feature-local** | Test features locally with cloud-first entity management | "test my changes", "run tests on localhost" |
| **publish-test-to-cloud** | Publish locally generated test scripts to cloud | "publish my test", "upload script to cloud" |

## Installation

### Get the Skills

Skills are available in the [muggle-ai-mcp repository](https://github.com/muggle-ai/muggle-ai-mcp).

```bash
git clone https://github.com/muggle-ai/muggle-ai-mcp.git
```

### For Cursor IDE

**Personal Installation** (available in all projects):

```bash
# Install local testing skills
cp -r muggle-ai-mcp/skills/local/* ~/.cursor/skills/
```

**Project Installation** (shared with team):

```bash
mkdir -p .cursor/skills
cp -r muggle-ai-mcp/skills/local/* .cursor/skills/
```

> **Important:** These skills work with local testing tools (`muggle-local-*` prefix) for `localhost` URLs. For public URLs, use remote skills with cloud QA tools (`muggle-remote-*` prefix).

## test-feature-local

Automates the complete local testing workflow with smart change detection.

### What It Does

```
Analyze Git Changes → Identify Impacted Features
        ↓
List/Create Project → List/Create Use Cases
        ↓
List/Create Test Cases → Check for Test Scripts
        ↓
Generate (if none) or Replay (if exists)
        ↓
Report Results Summary
```

### Example Usage

**Test based on code changes:**

```
You: "Test my changes"

Agent:
1. Analyzes git diff → finds LoginForm.tsx changed
2. Lists existing test cases → marks authentication tests as impacted
3. Asks which to run
4. Runs selected tests
5. Reports: "3 passed, 1 failed"
```

**Test a specific feature:**

```
You: "Test the login flow on localhost:3000"

Agent:
1. Creates project (if needed)
2. Creates use case and test case
3. Generates test script
4. Reports results
```

### Key Features

| Feature | Description |
| :------ | :---------- |
| **Change Detection** | Analyzes `git diff` to suggest impacted tests |
| **Batch Execution** | Run multiple tests with summary report |
| **Smart Matching** | Finds existing projects/use cases/test cases |
| **Auto-Create** | Creates missing entities as needed |

## publish-test-to-cloud

Publishes locally generated test scripts (action scripts) to Muggle AI cloud for replay, scheduling, and team collaboration.

### What It Does

```
Check Authentication → Login if Needed
        ↓
Find Local Run → Verify Cloud Test Case ID
        ↓
Upload Action Script → Link to Test Case
        ↓
Return Cloud URL
```

### Example Usage

**Publish after local test generation:**

```
You: "Publish my test run to the cloud"

Agent:
1. Checks auth → starts login flow if needed
2. Lists local run results → recommends most recent generation
3. User confirms the run to publish
4. Uploads action script to cloud
5. Reports: "Published! View at https://staging.muggle-ai.com/muggleTestV0/dashboard/projects/..."
```

**Publish specific run:**

```
You: "Publish run_1773735163358_roeot4 to cloud"

Agent:
1. Verifies run has valid cloudTestCaseId
2. Uploads action script
3. Links script to test case
4. Returns cloud URL
```

### Key Features

| Feature | Description |
| :------ | :---------- |
| **Device Code Auth** | Secure login flow via `muggle-remote-auth-*` |
| **Run Selection** | List and select from recent test generation runs |
| **Action Script Upload** | Uploads complete action script with screenshots |
| **Cloud URL** | Returns direct link to view published script |

## Skills vs Manual Tool Calls

| Approach | When to Use |
| :------- | :---------- |
| **Skills** | Common workflows, multi-step tasks, "just test it" |
| **Manual Tools** | Fine-grained control, specific operations, debugging |

Skills call the same tools under the hood—they just automate the workflow.

## Creating Custom Skills

Want to create your own testing skills? Skills are markdown files with:

1. **YAML frontmatter** — name and description
2. **Workflow steps** — what tools to call and when
3. **Decision logic** — how to handle different scenarios

See the [muggle-ai-mcp repository](https://github.com/muggle-ai/muggle-ai-mcp) for examples.

## Next Steps

- **[Tools Reference](tools-reference.md)** — Individual tool documentation
- **[Example Workflows](examples.md)** — See tools and skills in action
- **[Local Testing Setup](setup.md)** — Installation and configuration
