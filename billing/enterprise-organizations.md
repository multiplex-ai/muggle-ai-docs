# Enterprise Organizations

Muggle Test Enterprise is designed for companies that need centralized billing, team management, and shared resources across multiple projects and employees.

## Overview

An **Organization** is a company-level account that provides:

- **Centralized billing** — one subscription, one invoice, one payment method
- **Shared token wallet** — all members consume from the company's token pool
- **Seat-based licensing** — purchase seats and assign them to employees via email
- **Role-based access** — control what each team member can do
- **Org-wide projects** — projects belong to the organization, accessible by all members

## Getting Started

### Creating an Organization

1. Go to **Billing** in the dashboard
2. Select the **Enterprise** plan
3. Enter your company name and choose the number of seats (minimum 5)
4. Complete the Stripe checkout to activate your subscription
5. You'll be redirected to your new **Organization Dashboard**

You automatically become the **Owner** of the organization.

### Inviting Team Members

1. Go to **Organization** → **Members** in the sidebar
2. Click **Invite Member**
3. Enter the employee's email address and select their role:
   - **Admin** — can manage members, projects, and seats
   - **Member** — can use resources and run tests
4. Click **Send Invitation**

The invitee receives an email with a link to activate their account. If they don't already have a Muggle Test account, one is created for them automatically.

### Accepting an Invitation

When you receive an organization invitation email:

1. Click the **Accept Invitation** link
2. Sign in or create your Muggle Test account
3. You'll be added to the organization and redirected to the dashboard

Once you join, you'll have access to all organization projects and shared resources.

## Seats

Seats determine how many active members your organization can have.

| Detail | Description |
|--------|-------------|
| **Minimum seats** | 5 |
| **Price per seat** | $99/month |
| **Adding seats** | Organization → Seats → Add Seats |
| **Removing seats** | Takes effect at next billing period |

### Seat vs Member

| | Seat | Member |
|---|---|---|
| **What it is** | A purchased license slot | A person using a slot |
| **How it works** | You buy seats | You assign members to seats |
| **Limit** | Active members ≤ purchased seats | — |

If all seats are occupied, you must purchase additional seats before inviting more members.

## Roles

Organizations use a three-level role hierarchy:

| Role | Manage Members | Manage Projects | Manage Billing | View Usage | Use Resources |
|------|:-:|:-:|:-:|:-:|:-:|
| **Owner** | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Admin** | ✅ | ✅ | ❌ | ✅ | ✅ |
| **Member** | ❌ | ✅* | ❌ | ❌ | ✅ |

*Members can create and manage their own projects within the organization.

### Changing Roles

Only **Owners** and **Admins** can change member roles. There must always be at least one Owner.

### Removing Members

When a member is removed from the organization:
- Their access to all organization projects is revoked immediately
- Their individual Muggle Test account continues to work on the Free tier
- Projects they created remain in the organization

## Token Wallet

Enterprise organizations have a **shared token wallet** that all members consume from.

### How It Works

| Feature | Individual Plans | Enterprise |
|---------|-----------------|------------|
| **Token pool** | Per-user allocation | Shared org wallet |
| **Monthly limit** | 500K–20M depending on plan | No limit (pay per use) |
| **Billing** | Included in subscription | Metered usage billed monthly |
| **Cost** | — | $5 per 1,000,000 tokens |

When any org member runs a workflow (scan, test generation, replay), tokens are debited from the organization wallet — not from the member's individual balance.

### Monitoring Usage

Go to **Organization** → **Usage** to see:
- Total token consumption for the current billing period
- Usage breakdown by member
- Usage breakdown by project
- Historical usage trends

### Top-Up & Auto Top-Up

The org wallet can be topped up manually or configured with auto top-up:

1. Go to **Organization** → **Billing**
2. Click **Top Up Tokens** for a one-time purchase
3. Or configure **Auto Top-Up** to automatically purchase tokens when the balance drops below a threshold

## Projects

Projects created by organization members belong to the organization.

### Access

| Scenario | Who Can Access |
|----------|---------------|
| Org project | All org members (based on role) |
| Individual project | Only the project owner and explicitly granted users |

### Creating Projects

Any org member can create projects. Projects are automatically associated with the organization. The creator becomes the project owner within the org.

### Project Ownership

Organization projects have two levels of ownership:
1. **Organization** — the project belongs to the org and persists even if the creator leaves
2. **Project creator** — the individual who created the project, with project-level management rights

## Enterprise Quotas

Enterprise organizations have elevated limits:

| Resource | Enterprise Limit |
|----------|-----------------|
| **Projects** | Unlimited |
| **Use cases / project** | Unlimited |
| **Test cases / project** | Unlimited |
| **Concurrent workflows** | 20+ (configurable) |
| **Tokens** | Pay per use (no monthly cap) |
| **Seats** | As purchased (minimum 5) |

## Billing

### Subscription

Your enterprise subscription is billed monthly:
- **Seat fees**: $99 × number of seats
- **Token usage**: $5 per 1,000,000 tokens consumed

### Invoices

View invoices in **Organization** → **Billing** → **Billing History**.

### Cancellation

Only the organization **Owner** can cancel the subscription. Cancellation takes effect at the end of the current billing period. All members lose access to organization resources when the subscription ends.

## FAQ

### Can a user belong to multiple organizations?

Not currently. Each user can be a member of one organization at a time.

### What happens to my individual subscription when I join an organization?

Your individual subscription remains active but unused for resources consumed within the org. Token usage within the org is billed to the org wallet. You can cancel your individual subscription if you no longer need it.

### Can I transfer projects into an organization?

Not currently. Projects must be created within the organization. You can recreate individual projects as org projects.

### What happens if our token wallet runs out?

Workflows will be paused until the wallet is topped up. Enable auto top-up to prevent interruptions.

### How do I get a custom seat count or volume pricing?

Contact us at [sales@muggle-ai.com](mailto:sales@muggle-ai.com) for custom pricing on deployments with 20+ seats.

## Next Steps

| Goal | Resource |
|------|----------|
| Compare all plans | [Subscription Plans](subscription-plans.md) |
| Understand quotas | [Quotas and Limits](quotas-and-limits.md) |
| Get started with testing | [Quickstart](../getting-started/quickstart-running-tests.md) |
