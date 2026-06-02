# Testing Behind a Payment Wall

Test checkout against [Stripe test mode](https://docs.stripe.com/testing). Test mode uses the same flows as live mode but never moves real money, so you can run checkout on every build.

## Setup

| Step | Action                                                                                     |
| :--- | :----------------------------------------------------------------------------------------- |
| 1    | Point your app's environment at your Stripe **test** keys (`pk_test_…` / `sk_test_…`)       |
| 2    | Confirm the checkout page is in test mode — Stripe Elements shows a "TEST MODE" badge       |
| 3    | Tell Muggle Test which test card to use in your instruction                                |

> ⚠️ Never enter a real card number, even in test mode. Stripe rejects live cards on test keys, and real cards on a live key would create a real charge.

## Test Cards

Use any **future** expiry date, any 3-digit CVC (4 digits for American Express), and any postal/ZIP code.

**Successful payment**

| Card number           | Brand            |
| :-------------------- | :--------------- |
| `4242 4242 4242 4242` | Visa             |
| `5555 5555 5555 4444` | Mastercard       |
| `3782 822463 10005`   | American Express |
| `6011 1111 1111 1117` | Discover         |

**Declines and errors**

| Card number           | Result                                |
| :-------------------- | :------------------------------------ |
| `4000 0000 0000 0002` | Generic decline                       |
| `4000 0000 0000 9995` | Insufficient funds                    |
| `4000 0000 0000 0069` | Expired card                          |
| `4000 0000 0000 0127` | Incorrect CVC                         |
| `4000 0000 0000 0119` | Processing error                      |

**3D Secure / authentication required**

| Card number           | Result                                          |
| :-------------------- | :---------------------------------------------- |
| `4000 0025 0000 3155` | Prompts a 3D Secure authentication step         |
| `4000 0027 6000 3184` | Requires authentication on every transaction    |

Stripe maintains the full list — including bank-specific declines, dispute simulations, and test clocks for subscriptions — at [docs.stripe.com/testing](https://docs.stripe.com/testing).

## Example Instruction

> "Test checkout on `localhost:3000/cart`. Add the Pro plan, go to checkout, and pay with Stripe test card `4242 4242 4242 4242`, expiry `12/34`, CVC `123`, ZIP `12345`. Verify the order confirmation page appears."

To test the failure path, swap in a decline card and verify your app shows the right error:

> "Repeat checkout with declined card `4000 0000 0000 0002` and verify a 'Your card was declined' message appears and no order is created."

## Related

| Other gate                  | Guide                                                       |
| :-------------------------- | :--------------------------------------------------------- |
| App requires sign-in first  | [Testing Behind a Login Wall](testing-login-wall.md)       |
| Checkout shows a CAPTCHA    | [Testing Behind a CAPTCHA Wall](testing-captcha-wall.md)   |
| More example instructions   | [Local Testing Examples](local-testing/examples.md)        |
