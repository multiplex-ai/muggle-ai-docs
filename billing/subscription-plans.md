# Subscription Plans

Muggle Test offers different subscription tiers to match your testing needs. Every plan includes unlimited use cases and test cases per project; usage is metered in tokens.

For current pricing, see the [pricing page](https://muggle-ai.com/#pricing) or the product catalog API.

## Plan Comparison

| Feature | Free | Starter | Professional | Enterprise |
| :------ | :--- | :------ | :----------- | :--------- |
| **Concurrent Workflows** | 3 | 10 | 20 | 20+ |
| **Included Tokens** | 1M (one-time grant) | 3M / month | 20M / month | Pay per use ($5/M) |
| **Price** | $0 | $0 — verify your email to unlock | $199/month | $60/seat/month (min 4 seats) |
| **Seats** | 1 | 1 | 1 | As purchased (minimum 4) |
| **Shared team workspace** | — | — | — | Yes |
| **CI/CD Integration** | — | — | Yes | Yes |
| **Token Purchases** | — | Yes ($5/M) | Yes ($5/M) | Included (metered) |

## Concurrent Workflows

This controls how many workflows (scans, test generations, replays) can run simultaneously.

| Plan | Concurrent Limit |
| :--- | :--------------- |
| Free | 3 |
| Starter | 10 |
| Professional | 20 |
| Enterprise | 20+ |

A workflow started while you are at your limit waits for a running one to finish. See [Quotas and Limits](billing/quotas-and-limits.md) for how long it waits.

## Checking Your Current Usage

Open **Billing** in the dashboard to see your token balance and your concurrent workflow usage.

## Monthly Token Grants

The token allocation in your plan is credited to your wallet's **grant balance**. Here's how it behaves across each tier:

- **Free tier:** Free users receive a **one-time** 1M token grant at signup. This grant is **never refreshed** — when it's exhausted, move to Starter, which is available for all once you verify your email and refills to 3M tokens every month, or to a paid plan. There is no monthly reset for Free users.
- **Starter and Professional:** On every successful Stripe subscription invoice payment, your wallet's grant balance is **topped up to your plan amount**. If you have leftover grant from the previous cycle, it is **not** added to the new month's grant — instead, the grant balance is brought up to the plan amount (never reduced). Tokens you've purchased as top-ups are tracked separately and never expire.
- **Enterprise:** Enterprise organization wallets do **not** have a monthly grant bucket. Token usage is metered through Stripe at $5 per 1M tokens and reconciled on each invoice. The grant balance on an Enterprise org wallet is structurally always 0.
- **Plan upgrades:** When you upgrade mid-cycle (e.g., Starter → Professional), your wallet is credited the **difference** between the new and old plan amounts immediately on Stripe's proration invoice (e.g., upgrading from Starter to Pro mid-cycle credits 17M = 20M − 3M). Downgrades **never take tokens away**.
- **Token spending order:** When you use tokens, your **grant balance is drained first**, then any tokens you've purchased as top-up packages. This is fair to users who buy top-ups, since grant tokens may be reset on renewal anyway.

### Purchased token packages

If you need more tokens than your plan grants, you can buy a one-off token package on the billing page. **Tokens you buy this way never expire** and are tracked in a separate balance from your plan grant. They survive plan changes, renewals, and downgrades.

## Upgrading Your Plan

To increase your limits:

1. Go to **Billing** in the dashboard
2. Select the **Billing** tab
3. Click **Change Plan**
4. Choose your new plan

Changes take effect immediately, and new limits apply to all existing projects.

## Enterprise Tier

The Enterprise tier is built for teams. Instead of every engineer paying for their own subscription, your company gets an **organization** with a pool of seats, invites teammates, and shares a single token balance and a single set of projects. Enterprise is not available through self-serve checkout.

**Pricing**

- **$60 per seat per month**, with a minimum of **4 seats** ($240/month floor).
- **Tokens are pay-as-you-go at $5 per 1,000,000 tokens**, totaled at the end of each billing period and added to the same invoice. Enterprise organization wallets do not have a monthly grant bucket — all token consumption is purely pay-per-use, metered through Stripe.
- Seat count is adjustable. Added seats are prorated.

**What's included**

- 20+ concurrent workflows, raised on request
- Shared org wallet and pooled token usage
- Member roles (OWNER, ADMIN, MEMBER) for access control
- Cost attribution by member and by project
- CI/CD integration

**Personal accounts are unaffected.** Joining an organization does not change your personal subscription, your personal projects, or your personal wallet. The two are billed completely independently. See the [Enterprise Organizations guide](enterprise-organizations.md) for the full story, including how project ownership determines billing and how a single user can belong to more than one organization.

For volume discounts or a sales-assisted setup, contact us at [sales@muggle-ai.com](mailto:sales@muggle-ai.com).

## Next Steps

| Goal | Resource |
| :--- | :------- |
| View your current plan | [Billing Dashboard](https://www.muggle-ai.com) |
| Set up a team account | [Enterprise Organizations](enterprise-organizations.md) |
| Understand use cases | [How Muggle Test Works](concepts/how-muggle-test-works.md) |
| Run your first test | [Quickstart](getting-started/quickstart-running-tests.md) |
