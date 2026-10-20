# Enterprise Organizations

Muggle Test Enterprise lets a company run Muggle Test as a team. Instead of every engineer paying for their own subscription, your company gets an **organization** with a pool of seats, invites teammates, and shares a single token balance and a single set of projects across the team.

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

Seat count is adjustable after purchase; added seats are prorated for the rest of the billing cycle. The dashboard has no seat control yet — an OWNER sets the seat count through the organization API. To shrink, remove members first.

A pending invitation holds a seat until it is accepted or revoked. If active members and pending invitations already fill every seat, new invitations are rejected until seats are added or a pending invitation is revoked.

Runs on org projects draw down the organization's wallet and need a positive balance; when it reaches zero, runs on org projects stop. The dashboard cannot top up an organization wallet yet. Actual token consumption is totaled each billing period and added to your invoice alongside the seat subscription.

## Roles

An organization has three roles.

| Capability | OWNER | ADMIN | MEMBER |
| :--- | :---: | :---: | :---: |
| Invite and remove members | Yes | Yes | No |
| Change member roles | Yes | No | No |
| Create org projects | Yes | Yes | Yes |
| Manage all org projects | Yes | Yes | No |
| Billing contact (seats, payment method, invoices) | Yes | No | No |
| View usage by member and by project | Yes | Yes | No |
| Use org resources (run workflows, edit projects they have access to) | Yes | Yes | Yes |

The person who creates the organization is its first OWNER. Invitations offer the ADMIN or MEMBER role.

Organization roles also determine a baseline project role on every project the organization owns — for example, an Org OWNER is automatically a Project Owner on every org project, and an Org MEMBER is automatically a Project Viewer. Direct project grants stack on top of the baseline and only ever raise a user's access, never lower it. The full cascade is explained in [Organization Roles and Project Access](concepts/organization-roles-and-access.md).

### Project admin (project-level role)

When someone creates an org project, they automatically become **project admin** for that project. Project admin is a *project-level* role: it grants full control over that one project's settings, test cases, and collaborators, but it does **not** confer any org-level privileges.

OWNERs and ADMINs have project admin permissions on every org project, regardless of who created it. Other members get project-level access (admin, editor, viewer) on a per-project basis.

## Setting Up an Organization

Enterprise is not available through self-serve checkout. To start, choose **Contact sales** on the pricing page or in the dashboard's plan dialog.

The person who creates the organization, with your company name, becomes its first OWNER. A new organization starts with four seats. The OWNER then invites the rest of the team.

## Creating a Project Inside an Organization

A project associated with an organization is owned by the org rather than by you personally. That association drives everything else on this page: billing goes to the org wallet, enterprise limits apply, and every active member of the org gets a baseline project role via the [cascade](concepts/organization-roles-and-access.md).

The rules are:

- **You must be an active member of that organization.** Pending invitees and deactivated members cannot create projects on the org's behalf.
- **Any active member can do it.** OWNER, ADMIN, and MEMBER can all create org projects.
- **Personal → org is one-way.** An existing personal project can be moved into an organization you belong to, provided you can edit it. An org project can never move back to personal ownership or to another organization; recreate it under the new owner instead.
- **The cascade takes effect immediately.** Every current member of the org gets their cascade role on the project the moment it joins the org. New members get it the moment they accept their invitation.

The dashboard and the MCP tools cannot choose an organization yet. Set it through the projects API with the `organizationId` field, on create or on a later update.

## Inviting and Managing Members

OWNERs and ADMINs invite members from **Organization → Members**.

### Sending an invitation

1. Click **Invite Member**.
2. Enter the person's email address and choose their role (ADMIN or MEMBER).
3. Send the invitation.

The invitee receives an email containing the org name, your name as the inviter, the role they have been offered, and an activation link.

### Accepting an invitation

When the invitee clicks the link:

- If they already have a Muggle Test account, they sign in and confirm acceptance, then land on the org dashboard.
- If they do not yet have an account, they sign up first and then accept.

Once they accept, they become an active org member and occupy one seat.

### Invitation expiry

Invitation links expire **7 days** after they are sent. To replace an expired or lost invitation, an OWNER or ADMIN revokes it and sends a new one — revoking also frees the seat it was holding.

### When all seats are taken

If active members and pending invitations fill every seat, new invitations are rejected with "No available seats. Purchase more seats to invite new members." Revoke invitations you no longer need, or add seats (see [Pricing at a Glance](#pricing-at-a-glance)).

### Removing a member

OWNERs and ADMINs can remove a member from **Organization → Members**. Removal takes effect immediately, with no confirmation step. When a member is removed:

- Their membership in *this* org ends and the seat frees up immediately.
- They lose access to this org's projects.
- Their personal Muggle Test account is untouched, including any personal subscription, personal wallet, and personal projects.
- Any other organizations they belong to are unaffected.

## Belonging to More Than One Organization

A single Muggle Test user can belong to multiple organizations at the same time. Anyone can invite you to their org, even if you are already a member of another one — as long as a seat is available.

The **My organizations** page lists every organization you belong to, with your role and its status. The dashboard does not yet include an organization switcher: it opens the organization you most recently joined or created in the current browser.

## Personal vs. Org Billing

**Project ownership determines where bills go.** A project is personal unless it is associated with an organization, and once it belongs to an organization it stays there.

| | Personal project | Org project |
| :--- | :--- | :--- |
| **Owned by** | The user who created it | The organization |
| **Billed to** | The creator's personal wallet | The org's wallet |
| **Who can access** | The creator (and people they invite) | Org members, per project-level role |
| **Visible in** | The personal billing page | The org's usage and wallet history |

### A worked example

Sara is on a Pro plan personally. She joins her company's enterprise org as a MEMBER.

- Sara has an existing personal project, "weekend-side-project". It stays on her Pro plan and stays billed to her personal wallet. Joining the org changes nothing about it.
- Sara creates a new project for work, "checkout-flow-tests", and associates it with her organization. From that moment on, every workflow run against "checkout-flow-tests" debits the *org's* wallet.
- Sara also creates a small experimental personal project, "ai-experiment". Workflow runs against it debit Sara's personal wallet.
- Sara's company pays for the seat subscription and for any tokens that "checkout-flow-tests" (and other org projects) consume. Sara still pays for her personal Pro plan and for any tokens her personal projects consume.

The two accounts are independent. If Sara cancels her personal Pro plan, only her personal projects are affected. If the company cancels its enterprise subscription, only the org projects are affected.

OWNERs and ADMINs can see token usage broken down by member and by project through the organization usage API. The dashboard's organization Usage page is not available yet.

### Org Wallet Debits

Personal user wallets in Muggle Test split their token balance into two buckets: a **grant balance** (refilled monthly on subscription renewal for Starter/Pro users) and a **purchased balance** (top-up tokens that never expire). Debits drain the grant balance first, then fall through to purchased tokens.

**Organization wallets have no monthly grant.** Every token your org consumes is drawn from the wallet balance, and the consumption is billed on your next invoice at $5 per 1M tokens.

## Limits

Enterprise limits start at 50 use cases, 100 auto-detected use cases, and 300 test cases per project, and 20 concurrent workflows. They apply per member, to the account of whoever does the work. There is no limit on the number of projects. See [Quotas and Limits](billing/quotas-and-limits.md) for the full table.

## When a Payment Fails

If a scheduled payment on your organization fails — for example, because the card on file has expired — the organization is suspended immediately. There is no grace period. Every active OWNER receives a dunning email, and organization-scoped actions (running workflows, editing projects, inviting members) are refused with an "Organization is suspended" error until the bill is paid.

Members can still open the suspended organization in the dashboard and see its state. The dashboard does not have an organization billing page yet.

Recovery is automatic. As soon as a subsequent Stripe retry succeeds, the organization flips back to active and every blocked action starts working again — no support ticket, no manual reactivation.

The full rules live on [Billing Suspension and Recovery](billing/billing-suspension-and-recovery.md).

## Cancelling

Cancellation is not available in the dashboard yet. An OWNER can delete the organization through the organization API, which cancels the subscription immediately and deactivates every member. Personal accounts and personal projects belonging to former members are unaffected.

If you only need to *reduce* spend rather than cancel entirely, remove members and then lower the seat count to the number of remaining members, or to the four-seat minimum.

## Not Yet Supported

- **SSO / SAML.** Members sign in with their normal Muggle Test credentials. SSO is on the roadmap as a separate release.
- **Detailed audit logging.** Billing and status changes are recorded, but a full per-action audit trail is not yet available.
- **Moving an org project.** Org projects cannot move back to personal ownership or to another organization.
- **Self-serve organization management in the dashboard.** Seat changes, usage reports, cancellation, and choosing a project's organization are API-only; invoices, payment method, and wallet top-ups are not available; there is no organization switcher.

## Next Steps

| Goal | Resource |
| :--- | :--- |
| Understand how org roles cascade to project access | [Organization Roles and Project Access](concepts/organization-roles-and-access.md) |
| Learn what happens when a payment fails | [Billing Suspension and Recovery](billing/billing-suspension-and-recovery.md) |
| Compare enterprise to other plans | [Subscription Plans](billing/subscription-plans.md) |
| See enterprise limit details | [Quotas and Limits](billing/quotas-and-limits.md) |
| Talk to sales about a custom setup | [sales@muggle-ai.com](mailto:sales@muggle-ai.com) |
