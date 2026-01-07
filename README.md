# muggle-ai-docs

Muggle AI's customer-facing documentation repository for **Muggle Test** and related products.

This repo is consumed at runtime by the `muggle-prompt-ui` frontend, which:

- Loads `manifest.json` to build the docs sidebar and routing.
- Fetches individual markdown files referenced by `relativePath` in the manifest.

## Repository structure

- `manifest.json` – machine-readable index of sections and articles.
- `getting-started/` – onboarding and first-run guides.
- `concepts/` – deeper explanations of how Muggle Test works.
- `api/` – API usage, authentication, and examples.
- `troubleshooting/` – common issues and resolutions.

You can add more sections later as long as they are represented in `manifest.json`.

## Editing and adding documentation

1. **Add or update markdown files**
   - Create or edit `.md` files under the appropriate section folder.
   - Keep filenames descriptive and URL-friendly, for example:
     - `getting-started/overview.md`
     - `api/running-tests-via-api.md`

2. **Update `manifest.json`**
   - Each article must appear under a `section` entry with:
     - `articleId` – short identifier used in URLs.
     - `title` – display title in the UI.
     - `summary` – 1–2 sentence description used in search and lists.
     - `relativePath` – path to the markdown file from the repo root.
     - `order` – numeric sort order within the section.
   - Sections themselves have:
     - `sectionId`, `label`, and `order`.

3. **Push to the main branch**
   - The UI reads from the default branch (e.g. `main`) via a raw Git URL.
   - Once merged, changes are visible immediately in the docs UI.

## Manifest schema (high level)

```jsonc
{
  "version": "v0.1.0",
  "lastUpdatedAtUtc": "2025-01-01T00:00:00Z",
  "sections": [
    {
      "sectionId": "getting-started",
      "label": "Getting Started",
      "order": 1,
      "articles": [
        {
          "articleId": "overview",
          "title": "Muggle Test Overview",
          "summary": "High-level introduction to Muggle Test and when to use it.",
          "relativePath": "getting-started/overview.md",
          "order": 1,
          "tags": ["intro", "overview"]
        }
      ]
    }
  ]
}
```

When editing `manifest.json`, keep it valid JSON (no comments) and ensure every `relativePath` points to a real file.

 
