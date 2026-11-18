# Subscription Plans

Muggle Test offers different subscription tiers to match your testing needs. Every plan includes unlimited use cases and test cases per project; usage is metered in tokens.

For current pricing, see the [pricing page](https://muggle-ai.com/#pricing) or the product catalog API.

## Plan Comparison

| Feature | Free | Starter | Professional | Enterprise |
| :------ | :--- | :------ | :----------- | :--------- |
| **Concurrent Workflows** | 3 | 10 | 20 | 20 per concurrency block, pooled |
| **Included Tokens** | 2M (one-time grant) | 1.5M / month | 20M / month | None; all tokens prepaid |
| **Price** | $0 | $0 — verify your email to unlock | $199/month | $29/seat/month for seats 1–10, $24 for 11–50, $19 from 51 (minimum 3 seats), plus $99/month per block of 20 concurrent workflows |
| **Seats** | 1 | 1 | 1 | Bought ahead (minimum 3); members who only view use none |
| **Shared team workspace** | — | — | — | Yes |
| **CI/CD Integration** | — | — | Yes | Yes |
| **Token Purchases** | Yes ($5/M) | Yes ($5/M) | Yes ($5/M) | Prepaid ($5/M, from $100) |

## Concurrent Workflows

This controls how many workflows (scans, test generations, replays) can run simultaneously.

| Plan | Concurrent Limit |
| :--- | :--------------- |
| Free | 3 |
| Starter | 10 |
| Professional | 20 |
| Enterprise | 20 per concurrency block, shared by the whole organization |

A workflow started while you are at your limit waits for a running one to finish. See [Quotas and Limits](billing/quotas-and-limits.md) for how long it waits.

## Checking Your Current Usage

Open **Billing** in the dashboard to see your token balance and your concurrent workflow usage.

## Monthly Token Grants

The token allocation in your plan is credited to your wallet's **grant balance**. Here's how it behaves across each tier:

- **Free tier:** Free users receive a **one-time** 2M token grant at signup. This grant is **never refreshed** — when it's exhausted, move to Starter, which is available for all once you verify your email and refills to 1.5M tokens every month, or to a paid plan. There is no monthly reset for Free users.
- **Starter and Professional:** On every successful Stripe subscription invoice payment, your wallet's grant balance is **topped up to your plan amount**. If you have leftover grant from the previous cycle, it is **not** added to the new month's grant — instead, the grant balance is brought up to the plan amount (never reduced). Tokens you've purchased as top-ups are tracked separately and never expire.
- **Enterprise:** Organization wallets have **no** grant. The organization buys tokens in advance, and work on org projects draws that prepaid balance.
- **Plan upgrades:** When you upgrade mid-cycle (e.g., Starter → Professional), your wallet is credited the **difference** between the new and old plan amounts immediately on Stripe's proration invoice (e.g., upgrading from Starter to Pro mid-cycle credits 18.5M = 20M − 1.5M). Downgrades **never take tokens away**.
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

The Enterprise tier is built for teams. Your company gets an **organization** with shared projects, one prepaid token balance and one pool of concurrent workflows. Choose **Create organization** on the pricing page or the dashboard's **Billing** page, then subscribe on the organization's **Capacity** page; no sales call is needed.

**Pricing**

- **Contributor seats:** $29 per seat per month for seats 1–10, $24 for seats 11–50 and $19 from seat 51, minimum **3 seats**. Each Owner, Admin and Contributor uses a seat; members who only view use none.
- **Concurrency blocks:** $99 per month for each block of 20 concurrent workflows, minimum 1. Up to 3 blocks are self-serve; more through sales.
- **Tokens:** prepaid at $5 per 1,000,000 tokens, from $100 per purchase, with auto top-up. There is no monthly grant.
- Added seats and blocks apply at once and are prorated; reductions apply at the next renewal.

**What's included**

- One pool of concurrent workflows shared across the organization, with priority start when shared runners are busy
- Shared org projects and a prepaid org token balance
- Member roles (Owner, Admin, Contributor, Member) for access control
- Per-project concurrency and monthly token caps
- Usage by member and by project
- CI/CD integration

**Personal accounts are unaffected.** Joining an organization does not change your personal subscription, your personal projects, or your personal wallet. The two are billed completely independently. See the [Enterprise Organizations guide](billing/enterprise-organizations.md) for the full story, including how project ownership determines billing and how a single user can belong to more than one organization.

For more than 3 concurrency blocks or a sales-assisted setup, contact us at [sales@muggle-ai.com](mailto:sales@muggle-ai.com).

## Next Steps

| Goal | Resource |
| :--- | :------- |
| View your current plan | [Billing Dashboard](https://www.muggle-ai.com) |
| Set up a team account | [Enterprise Organizations](billing/enterprise-organizations.md) |
| Understand use cases | [How Muggle Test Works](concepts/how-muggle-test-works.md) |
| Run your first test | [Quickstart](getting-started/quickstart-running-tests.md) |
