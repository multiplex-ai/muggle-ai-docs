# Billing Suspension and Recovery

This page explains what happens to an enterprise organization when a scheduled payment fails, how your team is notified, what you can and cannot do while the organization is suspended, and how service is restored once the bill is paid.

The goal is to be explicit about the rules so Owners know exactly what to expect — and so Admins and Members who suddenly see "Organization is suspended" errors know what is going on and what to do about it.

If you are looking for general billing information, see [Enterprise Organizations](billing/enterprise-organizations.md) for how seats and pricing work, and [Subscription Plans](billing/subscription-plans.md) for how enterprise compares to other plans.

## Why Suspension Exists

An organization pays Stripe for two things: its seat subscription (a recurring charge per seat, billed monthly) and its token usage (metered, reconciled on the next invoice). Payments can fail for ordinary reasons — a card expires, a bank declines a transaction, a billing address does not match — and Muggle Test has to decide what to do while the account is unpaid.

The rule is deliberately simple: **when an invoice payment fails, the organization is suspended immediately**. There is no grace period, no reduced-service window, and no soft warning before the gate closes.

We made this choice for three reasons:

1. **Predictability.** Suspension is an unambiguous, binary state. You are either fully operational or fully suspended. There is no middle tier where "some features work and others don't" that ops has to explain on every support ticket.
2. **Honest attribution.** Continuing to serve traffic on an account that is not paying its bills means someone else is absorbing the cost. The simplest way to keep the product's pricing honest is to stop the meter the moment the bill stops being paid.
3. **Prompting a fast fix.** A small, visible outage is much more likely to get a card updated in the next five minutes than a quiet degradation would be. Suspension is designed to be immediately noticeable and immediately fixable.

## What "Suspended" Means in Practice

When Stripe tells Muggle Test that a scheduled payment on an organization has failed, three things happen at once:

1. **The organization is flipped to a suspended state.** Until the bill is paid, the organization is treated as unavailable.
2. **Every active Owner receives a dunning email.** The email names the organization, explains that a payment failed, and links directly to the organization's billing page so the Owner can update payment details and retry. Admins and Members are not emailed, on the assumption that billing is the Owner's responsibility and a wider blast just adds noise.
3. **Org-scoped API calls and workflows are refused.** Any request that needs organization context — running a workflow on an org project, editing an org project's test cases, inviting a new member, changing settings — is blocked with a clear error while the organization is suspended.

A blocked request returns a forbidden response with the message **"Organization is suspended."** This is the error Members and Admins are most likely to see first, and it is the signal to tell the Owner that billing needs attention.

### What still works while suspended

A suspended organization is not invisible. Two things remain reachable so the team can see what is going on and self-serve the fix:

- **Reading the organization itself.** Anyone with membership can still open the organization in the UI, see that it is in a suspended state, and see which Owners they should contact. Without this, a suspended org would simply "disappear" from the dashboard and nobody would know who to chase.
- **The organization's billing page.** Owners can view invoices, update payment methods, and retry the failed invoice from inside the product. Locking them out of billing while billing is broken would obviously be self-defeating.

Personal projects and personal accounts are completely unaffected. If Alice is a Member of a suspended org, her personal projects, her personal wallet, and any other organizations she belongs to continue working normally. Only the suspended organization is gated.

### What gets blocked

Everything else that requires organization context is blocked, including but not limited to:

| Area | Blocked while suspended? |
| :--- | :--- |
| Reading the organization record and billing page | **No, still allowed** (so you can see and fix the problem) |
| Running workflows against org projects | Yes |
| Creating, editing, or deleting org projects | Yes |
| Creating, editing, or deleting test cases or use cases on org projects | Yes |
| Inviting new members or changing member roles | Yes |
| Viewing cost and usage reports for org projects | Yes |
| Using the organization's shared token wallet | Yes |
| Personal projects belonging to any member | **Not blocked** (personal accounts are independent) |

## Dunning: How You Are Notified

Dunning is the word for the process of notifying customers about a failed payment and giving them a path to fix it. Muggle Test's dunning is intentionally minimal.

- **Who gets the email.** Every active Owner of the organization receives a dunning email. Owners who have been deactivated or removed are not emailed. Admins and Members are not emailed at all.
- **What the email says.** It names the organization, confirms that a Stripe-scheduled payment has failed, and includes a direct link to the organization's billing page where the Owner can update the payment method and retry.
- **How often it is sent.** The email is sent once per failure event — not once per Stripe retry attempt. Stripe may retry the same failed invoice several times over several days as part of its own dunning schedule; Muggle Test records each attempt in the subscription history, but it does not email Owners again on every retry. This keeps the inbox quiet and the signal clean: one email means "action needed," not "this is the fourth copy of the same alert."
- **What the email does not do.** It does not include sensitive billing details, it does not try to collect payment inline, and it does not ask for credentials. All it does is point you at the billing page.

### Why there is no grace period

A common question from new enterprise customers is "can you give us 24 or 48 hours before the gate closes?" The answer is no, and the reason is worth being explicit about:

A grace period is indistinguishable from the normal operating state to anyone who is not watching email — which is almost everyone on a team at any given moment. By the time the grace period expires and the org does lock, the people actually using the product have to drop what they are doing in the middle of their work. Cutting immediately is blunter, but it also produces a much faster fix, because the people experiencing the outage can escalate to the Owner directly.

If a longer notification window turns out to be important for a specific customer, raise it with the sales team.

## Auto-Recovery

Recovery is fully automatic. There is no "unsuspend" button and no support ticket required.

Stripe retries failed invoices on its own schedule. When one of those retries succeeds — whether that is an hour after the failure or several days later — the organization flips back to active immediately. Every blocked route lifts at the same time, the shared wallet becomes available again, and member and Owner actions start working exactly as they did before the suspension.

The recovery step is idempotent:

- **If the organization was suspended,** it is reactivated and an audit entry is recorded that the reactivation was triggered by a successful Stripe payment, not by a human operator.
- **If the organization was already active** — which is the case for every routine, healthy monthly payment — nothing happens. The event is simply a confirmation that the latest invoice was paid on time.

A few details worth knowing:

- **No recovery email is sent.** Owners already received the dunning email when the failure happened, and the product "just working again" speaks for itself. A second email purely to announce that things are working again would be noise. If you want confirmation, you can look at the subscription history on the billing page, which records both the failure and the successful recovery.
- **Members are never deactivated by a suspension.** Their seats stay assigned to them throughout the suspended period. As soon as the organization recovers, they continue working without having to be re-invited or re-enabled.
- **Pending invitations are not revoked.** An invitation that was valid before the suspension is still valid after recovery, unless the normal 7-day expiry has passed in the meantime.
- **Usage during the suspension is zero.** Because workflows and edits are blocked while the organization is suspended, there is no "usage that accumulated during the outage" to reconcile afterwards.

## Resolving a Suspension Yourself

If you are an Owner and your organization has just been suspended, the fastest path back is:

1. **Open the organization's billing page.** The link in the dunning email goes there directly; you can also navigate from the dashboard, since reading the org and its billing page is explicitly allowed during a suspension.
2. **Fix the cause of the failure.** Most often this is an expired card or a declined charge. Update the payment method on file, confirm the billing address, and save the change.
3. **Retry the failed invoice.** The billing page shows the failed invoice and exposes a retry action. You can trigger a retry manually, or wait for Stripe's next automatic retry — either will work.
4. **Confirm the organization is active again.** Once any retry succeeds, the organization flips back to active on its own. You will see it in the subscription history, and blocked routes will start working again immediately. No further action is required.

If none of that works — for example, the card keeps failing and you need to change providers — contact [sales@muggletest.com](mailto:sales@muggletest.com) and an operator can help you sort it out.

## Seat Count and Billing Stay in Sync

One subtle point about enterprise billing is that the number of seats on the organization record always matches the number of seats on the subscription in Stripe. You do not need to keep these in sync yourself:

- **Increasing seats** through the billing page updates Stripe first and then updates the organization, so the new capacity appears as soon as the change is confirmed.
- **Decreasing seats** similarly updates both. You cannot decrease seats below the number of currently active members; remove the members you do not need first.
- **External changes through Stripe** — for example, if a sales representative adjusts a quantity on the subscription directly — are reflected on the organization the next time Stripe notifies Muggle Test, which usually takes a few seconds.

The consequence you will actually notice is the invite limit. When every seat is occupied, new invitations are rejected with "No available seats. Purchase more seats to invite." Add seats from the billing page and try the invitation again.

Suspension does not change the seat count and does not free up seats. Members who were active when the organization was suspended are still counted against the seat cap, which is deliberate: they still belong to the organization and will be fully re-enabled the moment billing recovers.

## Related Pages

| Topic | Page |
| :--- | :--- |
| What an enterprise organization is, pricing, invitations | [Enterprise Organizations](billing/enterprise-organizations.md) |
| How organization roles translate into project access | [Organization Roles and Project Access](concepts/organization-roles-and-access.md) |
| Plan comparison | [Subscription Plans](billing/subscription-plans.md) |
| Enterprise quotas | [Quotas and Limits](billing/quotas-and-limits.md) |
