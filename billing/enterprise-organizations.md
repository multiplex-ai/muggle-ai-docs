# Enterprise Organizations

Muggle Test Enterprise lets a company run Muggle Test as a team. Instead of every engineer paying for their own subscription, you create an **organization**, buy a pool of seats, invite teammates, and share a single token balance and a single set of projects across the team.

This guide walks through how organizations work, how billing is split between personal and org accounts, and how to administer your org day to day.

## What is an Organization

An organization is a company account in Muggle Test that holds:

- A **subscription** with a configurable number of seats
- A **shared token wallet** that pays for workflow runs against org projects
- A set of **org-owned projects** that everyone in the org can collaborate on
- A **member list**, with roles that control who can do what

Each seat is assigned to one person on your team. Members join by accepting an email invitation. Once they accept, they share the org's projects, the org's token balance, and the org's usage visibility, according to their role.

Organizations live alongside personal accounts. Joining an org does not change anything about your personal Muggle Test account: you keep your personal projects, your personal subscription (if any), and your personal wallet. They are billed completely separately. See [Personal vs. org billing](#personal-vs-org-billing) for the full picture.

## Pricing at a Glance

| Item | Price |
| :--- | :---- |
| Seat subscription | **$60 / seat / month** |
| Minimum seats | **4** (so the floor is $240 / month) |
| Token usage | **$5 per 1,000,000 tokens**, billed at the end of each cycle as metered usage |

Seat count is adjustable after purchase. Adding seats takes effect immediately and is prorated for the rest of the billing cycle. You cannot reduce seats below the number of currently active members; remove members first if you need to shrink.

Token usage is pay-as-you-go. The org wallet does not need to be pre-funded with a fixed monthly bucket: actual token consumption across all org projects is totaled at the end of each billing period and added to your invoice alongside the seat subscription.

## Roles

An organization has three roles.

| Capability | OWNER | ADMIN | MEMBER |
| :--- | :---: | :---: | :---: |
| Invite and remove members | Yes | Yes | No |
| Create org projects | Yes | Yes | Configurable* |
| Manage all org projects | Yes | Yes | No |
| Manage billing (seats, payment method, invoices) | Yes | No | No |
| View org-wide usage | Yes | Yes | No |
| Use org resources (run workflows, edit projects they have access to) | Yes | Yes | Yes |

*Whether MEMBERs can create org projects is an org-level setting. It is enabled by default and can be turned off by an OWNER.

The user who creates the organization is its first OWNER.

### Project admin (project-level role)

When someone creates an org project, they automatically become **project admin** for that project. Project admin is a *project-level* role: it grants full control over that one project's settings, test cases, and collaborators, but it does **not** confer any org-level privileges.

OWNERs and ADMINs have project admin permissions on every org project, regardless of who created it. Other members get project-level access (admin, editor, viewer) on a per-project basis.

## Creating an Organization

Any Muggle Test user can create an organization. The flow is:

1. From the dashboard, open the org switcher and choose **Create Organization**.
2. Enter your company name.
3. Pick how many seats you want to start with (minimum 4).
4. Complete checkout for the seat subscription.

You become the OWNER of the new org as soon as checkout finishes. You can immediately start inviting members and creating org projects.

## Inviting and Managing Members

OWNERs and ADMINs invite members from **Organization Settings → Members**.

### Sending an invitation

1. Click **Invite Member**.
2. Enter the person's email address and choose their role (ADMIN or MEMBER).
3. Send the invitation.

The invitee receives an email containing the org name, your name as the inviter, the role they have been offered, and an activation link.

### Accepting an invitation

When the invitee clicks the link:

- If they already have a Muggle Test account, they sign in and confirm acceptance, then land on the org dashboard.
- If they do not yet have an account, they sign up first and then accept.

Once they accept, they become an active org member and consume one seat.

### Invitation expiry

Invitation links expire **7 days** after they are sent. If a link expires, the recipient sees an "Invitation expired" page with the option to request a new one. The OWNER or an ADMIN can re-send the invite at any time.

### When all seats are taken

If every seat is currently occupied, new invitations are blocked. You will see "No available seats. Purchase more seats to invite." Add seats from **Organization Settings → Billing**, then send the invitation again.

### Removing a member

OWNERs and ADMINs can remove a member from **Organization Settings → Members**. When a member is removed:

- Their membership in *this* org ends and the seat frees up immediately.
- They lose access to this org's projects.
- Their personal Muggle Test account is untouched, including any personal subscription, personal wallet, and personal projects.
- Any other organizations they belong to are unaffected.

## Belonging to More Than One Organization

A single Muggle Test user can belong to multiple organizations at the same time. Anyone can invite you to their org, even if you are already a member of another one — as long as a seat is available.

The dashboard includes an **org context switcher** at the top of the workspace. Use it to choose which organization (or your personal account) you are currently working in. The switcher controls:

- Which projects you see in the project list
- Which token balance and usage stats you see in the billing pages
- Which members and settings you can view

Switching context does not log you out and does not change your permissions in any other org — it just changes what you are looking at.

## Personal vs. Org Billing

This is the part that surprises most people, so it is worth spelling out carefully.

**Project ownership is chosen when the project is created and cannot be changed afterwards.** When you create a new project, you pick the owner: yourself (a personal project) or one of your organizations (an org project). That choice determines, for the entire life of the project, where its bills go and which quotas apply.

| | Personal project | Org project |
| :--- | :--- | :--- |
| **Owned by** | The user who created it | The organization |
| **Billed to** | The creator's personal wallet | The org's wallet |
| **Quotas** | The creator's personal plan | The org's enterprise quotas |
| **Who can access** | The creator (and people they invite) | Org members, per project-level role |
| **Concurrency budget** | Counts against the user's personal concurrency limit | Counts against the org's concurrency limit |
| **Visible in** | The personal billing page | The org billing page |

### A worked example

Sara is on a Pro plan personally. She joins her company's enterprise org as a MEMBER.

- Sara has an existing personal project, "weekend-side-project". It stays on her Pro plan and stays billed to her personal wallet. Joining the org changes nothing about it.
- Sara creates a new project for work, "checkout-flow-tests", and chooses her organization as the owner. From that moment on, every workflow run against "checkout-flow-tests" debits the *org's* wallet, and the project's use cases and test cases count against the org's quotas — not Sara's personal Pro quotas.
- Sara also creates a small experimental personal project, "ai-experiment", and chooses herself as the owner. Workflow runs against "ai-experiment" debit Sara's personal wallet and consume her personal Pro quotas.
- Sara's company pays for the seat subscription and for any tokens that "checkout-flow-tests" (and other org projects) consume. Sara still pays for her personal Pro plan and for any tokens her personal projects consume.

The two accounts are independent. If Sara cancels her personal Pro plan, only her personal projects are affected. If the company cancels its enterprise subscription, only the org projects are affected.

The cost-attribution view in the org billing page shows token usage broken down by member and by project, so OWNERs and ADMINs can see who and what is driving the bill.

## Quotas and Usage

Enterprise organizations get unlimited use cases, test cases, and projects, with reasonable starting caps that can be raised on request. Concurrency is configurable per org. See [Quotas and Limits](quotas-and-limits.md#enterprise-organizations) for the full table.

Quota enforcement follows project ownership: org-owned projects consume org quotas, and personal projects continue to consume the creator's personal plan quotas. Project counts and concurrency budgets are tracked separately for personal and org work.

## Cancelling

OWNERs can cancel the enterprise subscription from **Organization Settings → Billing**. The subscription stays active until the end of the current billing period. After cancellation:

- Members lose access to org projects.
- Org projects are no longer billable from the org wallet.
- Personal accounts and personal projects belonging to former members are unaffected.

If you only need to *reduce* spend rather than cancel entirely, remove members and then lower the seat count down to (but not below) the number of remaining members, or down to the four-seat minimum.

## Limitations / Not Yet Supported

To set expectations honestly, these capabilities are not yet part of the enterprise offering:

- **SSO / SAML.** Members sign in with their normal Muggle Test credentials. SSO is on the roadmap as a separate release.
- **Detailed audit logging.** Membership changes are recorded, but a full per-action audit trail is not yet available.
- **Changing a project's owner after creation.** Personal-vs-org ownership is fixed at project creation. If you need to "move" a project, recreate it under the desired owner.

## Next Steps

| Goal | Resource |
| :--- | :--- |
| Compare enterprise to other plans | [Subscription Plans](subscription-plans.md) |
| See enterprise quota details | [Quotas and Limits](quotas-and-limits.md#enterprise-organizations) |
| Talk to sales about a custom setup | [sales@muggletest.com](mailto:sales@muggletest.com) |
