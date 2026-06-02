# Testing Behind a Login Wall

Muggle Test signs in to protected areas using credentials you store on the project, then runs the rest of the flow as an authenticated user.

## Provide a Test Account

Use a dedicated test account — never a real user's credentials.

| Where you run | How to supply credentials                                                                 |
| :------------ | :---------------------------------------------------------------------------------------- |
| **Remote** (staging/production via MCP) | Save them under **Login Credentials** in your project settings, or store them as project secrets |
| **Local** (localhost) | Provide them in your instruction, or save them so the `autoLogin` preference can reuse them |

Stored credentials are reused across runs, so you set them once. The [`autoLogin` preference](local-testing/preferences.md) controls whether Muggle Test reuses saved credentials silently (`always`), asks first (`ask`, the default), or never reuses them (`never`).

## Example Instruction

> "Log in to `localhost:3000` as `test@example.com` / `Test1234!`, then open the billing page and verify the current plan is shown."

## SSO and Two-Factor Accounts

Automated sign-in cannot complete an interactive SSO redirect or a one-time 2FA code. For gated flows that normally require these:

| Situation                  | Recommendation                                                            |
| :------------------------- | :------------------------------------------------------------------------ |
| 2FA on the test account    | Disable 2FA on the test account, or use a test environment without it     |
| SSO-only login             | Point Muggle Test at a staging environment that allows password login     |
| Locked or expired account  | Reset or recreate the test account                                        |

If sign-in keeps failing, see [Login Keeps Failing](troubleshooting/common-issues.md#login-keeps-failing).

## Related

| Other gate                 | Guide                                                       |
| :------------------------- | :--------------------------------------------------------- |
| Paywall / checkout         | [Testing Behind a Payment Wall](testing-payment-wall.md)   |
| Login form shows a CAPTCHA | [Testing Behind a CAPTCHA Wall](testing-captcha-wall.md)   |
| More example instructions  | [Local Testing Examples](local-testing/examples.md)        |
