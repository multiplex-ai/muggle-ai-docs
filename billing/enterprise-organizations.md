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

The seat count on your organization always stays in sync with the quantity on the Stripe subscription, in both directions — changes you make inside Muggle Test are pushed to Stripe, and changes made on the Stripe side (for example, a sales rep adjusting a quantity) are reflected back on the organization the next time Stripe notifies Muggle Test. The practical effect is that the "seats remaining" count you see when inviting members is always authoritative. If every seat is occupied, new invitations are rejected until seats are added. See [Billing Suspension and Recovery → Seat Count and Billing Stay in Sync](billing/billing-suspension-and-recovery.md) for the full rules.

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

Organization roles also determine a baseline project role on every project the organization owns — for example, an Org OWNER is automatically a Project Owner on every org project, and an Org MEMBER is automatically a Project Viewer. Direct project grants stack on top of the baseline and only ever raise a user's access, never lower it. The full cascade is explained in [Organization Roles and Project Access](concepts/organization-roles-and-access.md), along with how to look up a member's effective role on any given project.

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

## Creating a Project Inside an Organization

When you create a project, you can associate it with an organization so that the project is owned by the org rather than by you personally. That association is the trigger for everything else on this page: billing goes to the org wallet, the project counts against enterprise quotas, and every active member of the org gets a baseline project role via the [cascade](concepts/organization-roles-and-access.md).

The rules for creating a project under an organization are:

- **You must be an active member of that organization.** Pending invitees and deactivated members cannot create projects on the org's behalf.
- **MEMBER is the minimum org role required.** Any active Owner, Admin, or Member can create an org project, unless the Owner has turned off member project creation in Organization Settings.
- **Project ownership is permanent.** You cannot later "move" a project between personal and org ownership, and you cannot move it from one organization to another. This is deliberate — it keeps billing and audit trails unambiguous over the project's whole lifetime. If you really need to move a project, recreate it under the new owner.
- **The cascade takes effect immediately.** Every current member of the org gets their cascade role on the project the moment it is created. New members will get the cascade role the moment they accept their invitation.

For the full cascade table (which org role maps to which project role) and for how direct project grants stack on top of the cascade, see [Organization Roles and Project Access](concepts/organization-roles-and-access.md).

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

### Org Wallet Debits

Personal user wallets in Muggle Test split their token balance into two buckets: a **grant balance** (refilled monthly on subscription renewal for Starter/Pro users) and a **purchased balance** (top-up tokens that never expire). Debits drain the grant balance first, then fall through to purchased tokens.

**Organization wallets share the same schema, but the grant bucket is structurally always 0** because Enterprise has no monthly token grant — it's purely pay-per-use at $5 per 1M tokens. In practice, every token your org consumes drains directly from the metered/purchased balance and is reconciled on the next Stripe invoice.

## Quotas and Usage

Enterprise organizations get unlimited use cases, test cases, and projects, with reasonable starting caps that can be raised on request. Concurrency is configurable per org. See [Quotas and Limits](billing/quotas-and-limits.md) for the full table.

Quota enforcement follows project ownership: org-owned projects consume org quotas, and personal projects continue to consume the creator's personal plan quotas. Project counts and concurrency budgets are tracked separately for personal and org work.

## When a Payment Fails

If a scheduled payment on your organization fails — for example, because the card on file has expired — the organization is suspended immediately. There is no grace period. Every active OWNER receives a dunning email with a direct link to the billing page, and organization-scoped actions (running workflows, editing projects, inviting members) are refused with an "Organization is suspended" error until the bill is paid.

You can still open the suspended organization in the UI and reach its billing page, which is how an OWNER fixes the payment and retries. Personal projects and other organizations are not affected.

Recovery is automatic. As soon as a subsequent Stripe retry succeeds, the organization flips back to active and every blocked action starts working again — no support ticket, no manual reactivation.

The full rules, including what exactly is blocked, how dunning email works, and why there is no grace period, live on [Billing Suspension and Recovery](billing/billing-suspension-and-recovery.md).

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
| Understand how org roles cascade to project access | [Organization Roles and Project Access](concepts/organization-roles-and-access.md) |
| Learn what happens when a payment fails | [Billing Suspension and Recovery](billing/billing-suspension-and-recovery.md) |
| Compare enterprise to other plans | [Subscription Plans](billing/subscription-plans.md) |
| See enterprise quota details | [Quotas and Limits](billing/quotas-and-limits.md) |
| Talk to sales about a custom setup | [sales@muggle-ai.com](mailto:sales@muggle-ai.com) |
