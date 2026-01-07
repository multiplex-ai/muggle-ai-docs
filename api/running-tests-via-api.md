# Running Tests via API

This guide shows how to trigger test runs and retrieve their results using the Muggle Test API.

## 1. Trigger a run

Send a `POST` request to the runs endpoint with the project and script information:

```http
POST /api/muggle-test/projects/{projectId}/runs
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "scriptIds": ["script-123", "script-456"],
  "runName": "Nightly regression"
}
```

### Request body fields

- `scriptIds` – list of test script identifiers to run.
- `runName` – optional label to help you recognize this run in the UI.

The response includes a `runId` that you can poll for status.

## 2. Poll run status

```http
GET /api/muggle-test/projects/{projectId}/runs/{runId}
Authorization: Bearer YOUR_API_KEY
```

The response contains:

- Current run status (queued, running, completed, failed).
- Per-script and per-step summaries.

## 3. Fetch reports

Once a run is completed, you can retrieve the detailed report via the reports endpoint (exact path may differ based on your backend version):

```http
GET /api/muggle-test/projects/{projectId}/runs/{runId}/report
Authorization: Bearer YOUR_API_KEY
```

Use this in CI/CD to:

- Fail the build when critical flows fail.
- Attach human-readable reports as build artifacts.


