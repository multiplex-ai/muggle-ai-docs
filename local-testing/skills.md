# Agent Skills for Local Testing

Pre-built AI agent skills that automate common testing workflows with Muggle AI.

## What are Agent Skills?

Agent Skills are structured instructions that teach AI assistants how to perform complex, multi-step workflows. Instead of manually calling individual tools, you can simply say "test my changes" and the agent follows the skill's workflow automatically.

## Local vs Remote Skills

The [muggle-ai-skills repository](https://github.com/muggle-ai/muggle-ai-skills) organizes skills by testing mode:

| Folder | MCP Package Mode | Use For |
| :----- | :--------------- | :------ |
| `local/` | `muggle-mcp serve --local` | `localhost`, local network |
| `remote/` | `muggle-mcp serve --qa` | Public URLs (staging, prod) |

Both modes are available with the unified `@muggleai/mcp` package. **This page covers local skills.** For remote testing, see [Remote Testing with MCP](../mcp/overview.md).

## Available Local Skills

| Skill | Description | Triggers |
| :---- | :---------- | :------- |
| **test-feature-local** | Test features locally with change detection | "test my changes", "run tests" |
| **publish-to-cloud** | Publish projects to Muggle AI cloud | "publish", "sync to cloud" |

## Installation

### Get the Skills

Skills are available in the [muggle-ai-skills repository](https://github.com/muggle-ai/muggle-ai-skills).

```bash
git clone https://github.com/muggle-ai/muggle-ai-skills.git
```

### For Cursor IDE

**Personal Installation** (available in all projects):

```bash
# Install local testing skills
cp -r muggle-ai-skills/local/* ~/.cursor/skills/
```

**Project Installation** (shared with team):

```bash
mkdir -p .cursor/skills
cp -r muggle-ai-skills/local/* .cursor/skills/
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

## publish-to-cloud

Publishes local test artifacts to Muggle AI cloud for team collaboration and production testing.

### What It Does

```
Check Authentication → Login if Needed
        ↓
Select Project → Update Production URL
        ↓
Sync to Cloud → Report Cloud URLs
```

### Example Usage

**First-time publish:**

```
You: "Publish my tests to the cloud"

Agent:
1. Checks auth → starts login flow if needed
2. Lists local projects → user selects one
3. Asks for production URL (localhost → production)
4. Syncs project, use cases, test cases
5. Reports: "Published! View at https://app.muggle-ai.com/projects/..."
```

**Sync updates:**

```
You: "Sync my project to the cloud"

Agent:
1. Finds project with existing cloud mapping
2. Syncs new/updated entities
3. Reports what changed
```

### Key Features

| Feature | Description |
| :------ | :---------- |
| **Device Code Auth** | Secure login flow |
| **URL Migration** | Prompts to update localhost → production URL |
| **Selective Publish** | Publish all, specific use case, or only new content |
| **Idempotent** | Re-publishing updates, doesn't duplicate |

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

See the [muggle-ai-skills repository](https://github.com/muggle-ai/muggle-ai-skills) for examples.

## Next Steps

- **[Tools Reference](tools-reference.md)** — Individual tool documentation
- **[Example Workflows](examples.md)** — See tools and skills in action
- **[Local Testing Setup](setup.md)** — Installation and configuration
