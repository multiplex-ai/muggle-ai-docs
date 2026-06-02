# Testing Behind a CAPTCHA Wall

Muggle Test detects CAPTCHAs during a run and solves the common types automatically, so login and sign-up flows complete without manual input.

| Type            | Supported |
| :-------------- | :-------: |
| reCAPTCHA v2    |     ✅     |
| reCAPTCHA v3    |     ✅     |
| hCaptcha        |     ✅     |
| Image CAPTCHAs  |     ✅     |
| Slider / audio  |     ❌     |

When a challenge can't be solved automatically, the run pauses and asks for help rather than failing silently.

> 💡 On staging, disable CAPTCHA where you can — runs are faster and more reliable. Keep it enabled in production tests to validate the real user experience.

For the full behavior, supported types, fallback flow, and limitations, see [Automated CAPTCHA Handling](automated-captcha-handling.md).

## Example Instruction

> "Sign up on `localhost:3000/signup` as a new user, completing the CAPTCHA if one appears, and verify the welcome screen loads."

## Related

| Other gate                | Guide                                                     |
| :------------------------ | :------------------------------------------------------- |
| Sign-in required          | [Testing Behind a Login Wall](testing-login-wall.md)     |
| Paywall / checkout        | [Testing Behind a Payment Wall](testing-payment-wall.md) |
| CAPTCHA handling in depth | [Automated CAPTCHA Handling](automated-captcha-handling.md) |
