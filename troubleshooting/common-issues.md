This page lists frequent issues and quick checks you can use to resolve them.

## Login keeps failing

Symptoms:

- Tests cannot get past the login page.
- Reports show repeated failures on the first step.

Checks:

- Double-check that the **base URL** is correct and reachable from the public internet.
- Verify credentials under **Login Credentials** are up to date.
- If you use SSO or 2FA, point Muggle Test at a staging environment without those enabled.

## Pages not discovered by Deep Scan

Symptoms:

- Important areas of your app never appear as candidate flows.

Checks:

- Make sure important flows are reachable by links or buttons from the base URL.
- Add additional **starting URLs** if needed (for example, `/app/dashboard`).
- Confirm that any feature flags or permissions needed for those pages are enabled on the test account.

## Tests are flaky

Symptoms:

- The same script sometimes passes and sometimes fails without code changes.

Checks:

- Look for heavy use of live data that changes between runs.
- Ensure long-running background jobs are not making the UI unstable.
- If you see explicit timeouts, consider simplifying the flow or reaching out to support with a failing run link.


