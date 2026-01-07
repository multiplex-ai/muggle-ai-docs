This guide walks you from zero to your first successful test run in a few minutes.

## 1. Create a project

1. Go to **Dashboard → Projects → New Project**.
2. Enter the **base URL** of the environment you want to test.
3. Optionally add **login credentials** so we can cover authenticated flows.

Recommendations:

- Use a **stable staging environment** where you can safely run tests.
- Disable 2FA and CAPTCHA on that environment so the agent can log in reliably.

## 2. Let Muggle Test explore your site

After creating the project:

- Start a **Deep Scan** to let the agent discover key user flows.
- You will see candidate flows such as _Login_, _Create Item_, _Edit Item_, _Search_, and dashboards.

You can:

- Rename flows to match your domain language.
- Disable flows you do not care about to keep reports focused.

## 3. Generate and run tests

1. Select the flows you care about most.
2. Click **Generate Tests**.
3. When scripts are ready, click **Run All**.

Muggle Test will:

- Open a real browser.
- Execute each step in the generated scripts.
- Capture screenshots and logs for debugging.

## 4. Review results

Once runs complete:

- Open the **Report** tab for the project.
- Filter by **status** (pass/fail) or by **flow name**.
- Drill down into failing steps to see:
  - The exact action that failed.
  - The page screenshot.
  - Any useful error messages.

From here you can fix issues in your app and re-run the affected tests to confirm the fix.


