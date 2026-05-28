# Quickstart by Role

Choose your role to see the fastest path to value with Muggle Test.

## Quick Navigation

| Role | Best For | Jump To |
| :--- | :------- | :------ |
| **Developers** | Build locally, test in CI/CD, ship with confidence | [Developer Guide](#developers) |
| **Vibe Coders** | Prompt-driven development with preview URL testing | [Vibe Coder Guide](#vibe-coders) |
| **Product Team** | PRD-based test generation and triage workflows | [Product Team Guide](#product-team) |

**Common Resources:** [Investigating Failures](#investigating-failures) | [Managing Discovered Flows](#managing-discovered-flows) | [Reviewing Results](#reviewing-results) | [Next Steps](#next-steps)

---

## Developers

**Your workflow:** Build features → Test locally → Fix issues → Release with confidence

### Recommended Path

| Step | Action | Guide |
| :--- | :----- | :---- |
| 1 | Install Muggle Test Local for your AI assistant | [Local Testing Setup](local-testing/setup.md) |
| 2 | Test your localhost app while coding | [Local Testing Examples](local-testing/examples.md) |
| 3 | Fix issues from the test report | [Investigating Failures](#investigating-failures) |
| 4 | Create a project for your staging environment | [Running Your First Tests](quickstart-running-tests.md) |
| 5 | Integrate into CI/CD for automated regression | [CI/CD Integration](mcp/mcp-cicd-integration.md) |

### Key Tools

- **Local testing**: Test localhost directly from Cursor, Claude, or VS Code
- **MCP integration**: Let your AI assistant run tests as you code
- **CI/CD hooks**: Block deploys when critical journeys fail

---

## Vibe Coders

**Your workflow:** Prompt for features → Validate in cloud → Fix quickly → Publish with confidence

### Recommended Path

| Step | Action | Guide |
| :--- | :----- | :---- |
| 1 | Connect Muggle Test to your AI assistant | [MCP Quickstart](mcp-quickstart.md) |
| 2 | Ask your AI to test your preview URL | [Local Testing Examples](local-testing/examples.md) |
| 3 | Review the report and fix issues | [Investigating Failures](#investigating-failures) |
| 4 | Re-test until checks pass | Repeat step 2 |

### Key Tools

- **MCP skills**: Just say "test my app" and your AI handles the rest
- **Preview URL testing**: Works with Vercel, Netlify, Cloudflare previews
- **Fast feedback loop**: Results in minutes, not hours

### Example Prompts

```
"Test my Vercel preview at https://my-app-abc123.vercel.app"

"Run Muggle Test on my staging site and tell me what's broken"

"Check if the signup flow works on localhost:3000"
```

---

## Product Team

**Your workflow:** Import requirements → Generate tests → Review findings → Triage fixes

### Recommended Path

| Step | Action | Guide |
| :--- | :----- | :---- |
| 1 | Create a project for your staging environment | [Running Your First Tests](quickstart-running-tests.md) |
| 2 | Upload your PRD or requirements document | [Uploading PRD Files](concepts/prd-upload.md) |
| 3 | Review and approve generated use cases | [Managing Discovered Flows](#managing-discovered-flows) |
| 4 | Run tests and review the report | [Review Results](#reviewing-results) |
| 5 | Triage issues by severity and assign to developers | [Investigating Failures](#investigating-failures) |

### Key Tools

- **PRD upload**: Extract test scenarios from your existing documentation
- **AI-generated use cases**: Describe what you want to test in plain language
- **Shareable reports**: Export findings to Jira, Linear, or Slack

### What You Can Upload

| Format | Best For |
| :----- | :------- |
| PDF | Product specs, design docs |
| DOCX | Requirements documents, user stories |
| TXT | Plain text specifications |

---

## Common Reference

### Investigating Failures

For each failing test, you can see:

| Information | Purpose |
| :---------- | :------ |
| **Action** | Exact step that failed |
| **Screenshot** | Visual state at failure time |
| **Error message** | Technical details for debugging |
| **Page URL** | Where the failure occurred |

### Managing Discovered Flows

| Action | When to Use |
| :----- | :---------- |
| **Rename** | Match your domain language |
| **Disable** | Exclude flows you don't care about |
| **Prioritize** | Mark critical business flows |

### Reviewing Results

Use filters to focus on what matters:

| Filter | Use Case |
| :----- | :------- |
| **Status** | Show only passed or failed tests |
| **Flow name** | Focus on specific user journeys |
| **Severity** | Prioritize high-impact issues |

---

## Next Steps

| Goal | Resource |
| :--- | :------- |
| Understand how Muggle Test works | [How Muggle Test Works](concepts/how-muggle-test-works.md) |
| Set up automated testing | [CI/CD Integration](mcp/mcp-cicd-integration.md) |
| Learn about quotas and pricing | [Subscription Plans](billing/subscription-plans.md) |
| Fix a problem | [Common Issues](troubleshooting/common-issues.md) |
