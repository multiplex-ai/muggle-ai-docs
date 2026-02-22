# Common Issues & Fixes

This page lists frequent issues and quick checks to resolve them.

## Login Keeps Failing

**Symptoms:**

- Tests cannot get past the login page
- Reports show repeated failures on the first step

**Diagnosis & Solutions:**

| Check                | Solution                                               |
| :------------------- | :----------------------------------------------------- |
| Base URL correct?    | Verify URL is reachable from public internet           |
| Credentials current? | Update under **Login Credentials** in project settings |
| SSO/2FA enabled?     | Point to staging environment without these             |
| Account locked?      | Reset test account or create new one                   |

## Pages Not Discovered by Deep Scan

**Symptoms:**

- Important areas of your app never appear as candidate flows
- Critical features missing from discovered scenarios

**Diagnosis & Solutions:**

| Check                    | Solution                                     |
| :----------------------- | :------------------------------------------- |
| Pages reachable?         | Ensure links/buttons lead to important flows |
| Need more starting URLs? | Add additional URLs (e.g., `/app/dashboard`) |
| Permissions missing?     | Enable feature flags for test account        |
| Behind authentication?   | Provide valid credentials                    |

## Tests Are Flaky

**Symptoms:**

- Same script sometimes passes, sometimes fails
- No code changes between runs

**Diagnosis & Solutions:**

| Check                     | Solution                              |
| :------------------------ | :------------------------------------ |
| Live data changing?       | Use stable test data or seed data     |
| Background jobs running?  | Ensure UI is stable during tests      |
| Timing issues?            | Contact support with failing run link |
| Third-party dependencies? | Mock or stub external services        |

## API Authentication Errors

**Symptoms:**

- `401 Unauthorized` responses
- `403 Forbidden` responses

**Diagnosis & Solutions:**

| Check                  | Solution                                            |
| :--------------------- | :-------------------------------------------------- |
| API key correct?       | Verify key matches dashboard                        |
| Key expired?           | Regenerate in **Settings → API Keys**               |
| Wrong environment?     | Check key matches target environment                |
| Header format correct? | Use `Authorization: Bearer KEY` or `x-api-key: KEY` |

## Timeouts During Test Runs

**Symptoms:**

- Tests never complete
- Status stuck on `running`

**Diagnosis & Solutions:**

| Check            | Solution                            |
| :--------------- | :---------------------------------- |
| Site responding? | Verify target environment is up     |
| Too many tests?  | Run smaller batches                 |
| Network issues?  | Check firewall/proxy settings       |
| Heavy pages?     | Simplify flows or increase timeouts |

## Reports Not Generating

**Symptoms:**

- Report tab shows no data
- API returns empty report

**Diagnosis & Solutions:**

| Check              | Solution                                |
| :----------------- | :-------------------------------------- |
| Run completed?     | Wait for `completed` or `failed` status |
| Permissions?       | Verify account has report access        |
| Recent runs exist? | Run tests first before viewing reports  |

## CAPTCHA Blocking Tests

**Symptoms:**

- Tests fail on pages with CAPTCHA challenges
- Agent requests user intervention for CAPTCHA

**Diagnosis & Solutions:**

| Check                       | Solution                                                              |
| :-------------------------- | :-------------------------------------------------------------------- |
| Supported CAPTCHA type?     | See [Automated CAPTCHA Handling](concepts/automated-captcha-handling.md) for supported types |
| Slider or audio CAPTCHA?    | These are not yet supported; consider testing on staging without them |
| New CAPTCHA implementation? | Contact support with details about the CAPTCHA type                   |
| Frequent CAPTCHA triggers?  | Your test account may be flagged; try a new account or whitelist IP   |

**Tip:** If possible, disable CAPTCHAs on your staging environment for faster, more reliable test execution.

## Getting More Help

If these solutions don't resolve your issue:

| Resource                  | When to Use                     |
| :------------------------ | :------------------------------ |
| **In-app chat**           | Quick questions, account issues |
| **support@muggle-ai.com** | Detailed technical issues       |
| **Include run link**      | Helps us investigate faster     |When contacting support, include:- Project ID
- Run ID (if applicable)
- Steps to reproduce
- Screenshots or error messages
