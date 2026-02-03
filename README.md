# Muggle AI Documentation

Muggle AI's customer-facing documentation repository for **Muggle Test** and related products.

This repo is consumed at runtime by the `muggle-prompt-ui` frontend, which:

- Loads `manifest.json` to build the docs sidebar and routing
- Fetches individual markdown files referenced by `relativePath` in the manifest

## Repository Structure

| Folder | Contents |
|:-------|:---------|
| `getting-started/` | Onboarding and first-run guides |
| `concepts/` | Deeper explanations of how Muggle Test works |
| `api/` | API usage, authentication, and examples |
| `mcp/` | MCP Gateway documentation for AI assistant integration |
| `troubleshooting/` | Common issues and resolutions |

## Editing and Adding Documentation

### 1. Add or Update Markdown Files

Create or edit `.md` files under the appropriate section folder.

| Guideline | Example |
|:----------|:--------|
| Descriptive filenames | `getting-started/overview.md` |
| URL-friendly names | `api/running-tests-via-api.md` |
| Follow doc standards | See `.cursor/rules/documentation-standards.mdc` |

### 2. Update `manifest.json`

Each article must appear under a `section` entry:

| Field | Description |
|:------|:------------|
| `articleId` | Short identifier used in URLs |
| `title` | Display title in the UI |
| `summary` | 1-2 sentence description for search/lists |
| `relativePath` | Path to markdown file from repo root |
| `order` | Numeric sort order within section |
| `tags` | Keywords for search |

Sections have:

| Field | Description |
|:------|:------------|
| `sectionId` | Unique section identifier |
| `label` | Display name in sidebar |
| `order` | Sort order among sections |

### 3. Push to Main Branch

The UI reads from the default branch (e.g., `main`) via raw Git URL. Changes are visible immediately after merge.

## Manifest Schema

```jsonc
{
  "version": "v0.2.0",
  "lastUpdatedAtUtc": "2026-02-01T00:00:00Z",
  "sections": [
    {
      "sectionId": "getting-started",
      "label": "Getting Started",
      "order": 1,
      "articles": [
        {
          "articleId": "overview",
          "title": "Muggle Test Overview",
          "summary": "High-level introduction to Muggle Test.",
          "relativePath": "getting-started/overview.md",
          "order": 1,
          "tags": ["intro", "overview"]
        }
      ]
    }
  ]
}
```

> **Note**: Keep `manifest.json` as valid JSON (no comments) and ensure every `relativePath` points to a real file.

## Documentation Standards

This repo follows specific standards for customer-facing documentation:

| Standard | Requirement |
|:---------|:------------|
| **Diagrams** | Use Mermaid, not ASCII art |
| **Tables** | Use proper alignment (`:---` left, `---:` right, `:---:` center) |
| **Security** | Never expose internal details or sensitive information |
| **Style** | Use second person ("you"), keep sentences concise |

See `.cursor/rules/documentation-standards.mdc` for full guidelines.
