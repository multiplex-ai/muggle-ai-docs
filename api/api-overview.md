# API Overview

You can trigger runs and fetch results programmatically using the Muggle Test API.

## Use Cases

| Scenario | How the API Helps |
|:---------|:------------------|
| CI/CD integration | Trigger tests on every deploy |
| Smoke tests | Run critical tests before/after releases |
| Custom dashboards | Build reporting on top of test results |
| Scheduled runs | Automate nightly regression tests |

## Authentication

All API requests require authentication via a **Bearer token** or **API key**.

### Bearer Token

```http
Authorization: Bearer YOUR_API_KEY
```

### API Key Header

```http
x-api-key: YOUR_API_KEY
```

### Getting Your API Key

1. Go to **Dashboard → Settings → API Keys**
2. Click **Create API Key**
3. Copy and store securely (format: `mai_sk_...`)

> **Security**: Never commit API keys to version control or expose them in client-side code.

## Base URL

```text
https://api.muggle-ai.com
```

All endpoints in this documentation are relative to this base URL.

## Common Patterns

| Method | Purpose | Example |
|:-------|:--------|:--------|
| `GET` | Retrieve entities | List projects, fetch runs |
| `POST` | Create or trigger | Start a test run |
| `PUT` | Update entities | Modify project settings |
| `DELETE` | Remove entities | Delete a test script |

## Request/Response Format

| Aspect | Standard |
|:-------|:---------|
| Content type | `application/json` |
| Character encoding | UTF-8 |
| Date format | ISO 8601 (`2024-01-15T10:30:00Z`) |

## Error Handling

| Status Code | Meaning | Action |
|:-----------:|:--------|:-------|
| 200 | Success | Process response |
| 400 | Bad request | Check request parameters |
| 401 | Unauthorized | Verify API key |
| 403 | Forbidden | Check permissions |
| 404 | Not found | Verify resource ID |
| 429 | Rate limited | Back off and retry |
| 500 | Server error | Retry with exponential backoff |

## Rate Limits

| Tier | Requests/minute |
|:-----|----------------:|
| Free | 20 |
| Pro | 60 |
| Enterprise | 300 |

Rate limit headers are included in responses:

| Header | Description |
|:-------|:------------|
| `X-RateLimit-Limit` | Maximum requests allowed |
| `X-RateLimit-Remaining` | Requests remaining |
| `X-RateLimit-Reset` | Reset time (Unix timestamp) |

## Next Steps

| Goal | Resource |
|:-----|:---------|
| Trigger test runs | [Running Tests via API](./running-tests-via-api.md) |
| Use AI assistants | [MCP Gateway](../mcp/overview.md) |
| CI/CD integration | [CI/CD Integration](../mcp/mcp-cicd-integration.md) |
