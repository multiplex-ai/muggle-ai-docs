# How Muggle Test Works

At a high level, Muggle Test does three things:

1. **Discover** important flows in your app.
2. **Generate** executable test scripts from those flows.
3. **Re-run** those scripts on every change and surface regressions.

## Discovery

Muggle Test uses an AI agent in a real browser to:

- Crawl your site starting from the base URL you provide.
- Identify key pages and actions (login, search, forms, dashboards, etc.).
- Propose flows as labeled scenarios in your project.

The agent is optimized for:

- Navigating realistic multi-step workflows.
- Handling dynamic content and asynchronous loading.

## Test scripts

Each scenario is turned into a sequence of concrete steps, for example:

- “Click **Sign in** button.”
- “Type email into field labeled **Email**.”
- “Wait for dashboard to finish loading.”

Muggle Test keeps scripts as stable as possible by:

- Preferring human-visible labels over brittle CSS selectors.
- Allowing for minor layout and content changes.

## Runs and reports

When you run tests:

- Scripts are executed in a browser environment.
- Each step is evaluated as pass/fail.
- Failures capture enough context to debug quickly (URL, step description, screenshot, and summary).

Reports help you:

- See which flows are currently healthy.
- Prioritize fixes by business impact.
- Share understandable results with non-engineers.


