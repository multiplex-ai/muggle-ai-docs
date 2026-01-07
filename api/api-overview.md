You can trigger runs and fetch results programmatically using the Muggle Test API.

This is useful for:

- Integrating with CI/CD pipelines.
- Running smoke tests before or after deployments.
- Building custom dashboards on top of test results.

## Authentication

All API requests are authenticated with a **Bearer token**:

```http
Authorization: Bearer YOUR_API_KEY
```

You can create and manage keys in **Dashboard → Settings → API Keys**.

## Base URL

Your API base URL is typically the same as the backend URL used by the UI, for example:

```text
https://api.muggle-ai.com
```

All endpoints in this documentation are described relative to that base URL.

## Common patterns

- Use **`GET`** requests to retrieve entities (projects, scripts, runs, reports).
- Use **`POST`** requests to create entities or trigger actions (start a run).
- Handle **rate limiting** and **transient failures** with retries where appropriate.

Detailed endpoint examples are provided in the specific API articles (for example, _Running Tests via API_).


