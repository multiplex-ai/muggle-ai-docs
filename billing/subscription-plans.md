# Subscription Plans

Muggle Test offers different subscription tiers to match your testing needs. Each plan includes specific quotas for use cases, test cases, and concurrent workflow execution.

## Plan Comparison

| Feature | Free | Starter | Professional | Enterprise |
| :------ | :--- | :------ | :----------- | :--------- |
| **Journeys Scanned** | 10 | 30 | 100 | Unlimited |
| **Use Cases / Project** | 3 | 10 | 50 | Unlimited |
| **Test Cases / Project** | 5 | 30 | 300 | Unlimited |
| **Concurrent Workflows** | 1 | 3 | 10 | 20+ |
| **Monthly Tokens** | 500K | 3M | 20M | Pay per use |
| **Projects** | 1 | 3 | Unlimited | Unlimited |
| **Seats** | 1 | 1 | 1 | 5+ (per-seat) |
| **Organization Account** | — | — | — | ✅ |
| **Shared Token Wallet** | — | — | — | ✅ |

## Understanding Quotas

### Journeys Scanned

When you run a website scan, Muggle Test's AI agent discovers potential use cases (journeys). The scan limit determines how many journeys can be detected per scan.

| Plan | Journeys Limit |
| :--- | :------------- |
| Free | 20 per scan |
| Professional | 50 per scan |
| Enterprise | 50+ (customizable) |

### Use Cases

Use cases represent the high-level user journeys you want to test (e.g., "User Login", "Add to Cart", "Checkout"). Each use case can have multiple test cases.

| Plan | Use Case Limit |
| :--- | :------------- |
| Free | 3 per project |
| Professional | 20 per project |
| Enterprise | 100+ |

### Test Cases

Test cases are the specific test scenarios generated from use cases. Each use case typically generates multiple test cases covering different paths and edge cases.

| Plan | Test Case Limit |
| :--- | :-------------- |
| Free | 10 per project |
| Professional | 60 per project |
| Enterprise | 300+ |

### Concurrent Workflows

This controls how many workflows (scans, test generations, replays) can run simultaneously.

| Plan | Concurrent Limit |
| :--- | :--------------- |
| Free | 3 |
| Professional | 10 |
| Enterprise | 50+ (customizable) |

## What Happens When You Hit a Limit

### Use Case Limit

When you reach your use case limit:

| Scenario | Behavior |
| :------- | :------- |
| **Manual creation** | You'll see an error message and cannot create more use cases |
| **Auto-detection approval** | Only the first N candidates (up to your remaining quota) will be approved |
| **Existing use cases** | Continue to work normally |

### Test Case Limit

When you reach your test case limit:

| Scenario | Behavior |
| :------- | :------- |
| **Manual creation** | You'll see an error message and cannot create more test cases |
| **Auto-detection workflow** | The workflow will create test cases up to your limit and stop |
| **Existing test cases** | Continue to work normally |

### Partial Success

For batch operations (like approving multiple use case candidates), Muggle Test uses **partial success**:

- Items up to your remaining quota are processed
- Excess items are skipped
- You receive a message indicating how many were processed vs. skipped

## Checking Your Current Usage

### In the Dashboard

1. Navigate to your project
2. Click **Add Use Cases** to open the candidates modal
3. View your quota status: "Use Cases: X/Y"

Hover over the quota display to see:
- Current use case count in the project
- Your plan's limit
- Available slots remaining

### Via API

Use the use case discovery memory endpoint to get quota information:

```
GET /api/muggleTest/projects/{projectId}/useCaseDiscoveryMemory
```

Response includes:
```json
{
  "memory": { ... },
  "useCaseQuota": {
    "currentUsage": 5,
    "limit": 10,
    "remaining": 5,
    "isExempt": false
  }
}
```

## Upgrading Your Plan

To increase your limits:

1. Go to **Billing** in the dashboard
2. Select the **Billing** tab
3. Click **Change Plan**
4. Choose your new plan

Changes take effect immediately, and new limits apply to all existing projects.

## Enterprise Plan

Enterprise is designed for companies that need centralized billing, team management, and shared resources.

| Feature | Details |
| :------ | :------ |
| **Pricing** | $99/seat/month (minimum 5 seats) |
| **Token billing** | Pay per use — $5 per 1,000,000 tokens |
| **Organization account** | Centralized company account with admin controls |
| **Seats** | Purchase and assign seats to employees via email invitation |
| **Shared token wallet** | All members consume from one company wallet |
| **Roles** | Owner, Admin, Member — control access and permissions |
| **Projects** | Unlimited, belong to the organization |
| **Concurrent workflows** | 20+ (configurable) |

### Key Enterprise Features

- **Seat management** — purchase seats and invite employees by email
- **Shared billing** — one subscription, one invoice, one payment method for the whole team
- **Usage analytics** — see token consumption by member and by project
- **Role-based access** — Owners manage billing, Admins manage members, Members use resources

For full details, see [Enterprise Organizations](enterprise-organizations.md).

For custom pricing on deployments with 20+ seats, contact [sales@muggle-ai.com](mailto:sales@muggle-ai.com).

## Next Steps

| Goal | Resource |
| :--- | :------- |
| View your current plan | [Billing Dashboard](/muggleTestV0/dashboard/billing) |
| Understand use cases | [How Muggle Test Works](../concepts/how-muggle-test-works.md) |
| Run your first test | [Quickstart](../getting-started/quickstart-running-tests.md) |
