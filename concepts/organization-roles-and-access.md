# Organization Roles and Project Access

This page explains how organization membership translates into access on the projects an organization owns, and how Muggle Test answers the question "why does this person have that role on this project?" when you need to audit permissions.

If you are new to enterprise organizations, start with [Enterprise Organizations](billing/enterprise-organizations.md) — this page assumes you already know what an org is, how seats work, and what personal vs. org billing means.

## Why This Page Exists

Access inside an organization comes from more than one place:

- You might have been invited to a specific project directly.
- You might belong to a team that has been given access to a project.
- You might have created the project, which makes you its implicit owner.
- You might be a member of the organization that owns the project, in which case your organization role grants you a baseline level of access on every project the org owns.

When more than one of these is true at the same time, Muggle Test resolves them into a single **effective role** for that person on that project. This page describes how that resolution works so you can reason about team access without having to guess.

## Organization Roles at a Glance

An organization has three roles:

| Organization role | Intent |
| :--- | :--- |
| **Owner** | Runs the organization. Controls billing, can invite and remove anyone, has full control over every org project. |
| **Admin** | Day-to-day operator. Manages members and projects, but does not touch billing and cannot delete projects outright. |
| **Member** | Regular org user. Can work inside projects they have been given access to, and gets read-only visibility into every org project by default. |

See [Enterprise Organizations → Roles](billing/enterprise-organizations.md) for the full capability matrix at the organization level (who can invite members, who can manage billing, and so on).

## How Organization Roles Cascade to Projects

Every project owned by an organization automatically grants a baseline project role to every active member of that organization, based on their organization role:

| If your organization role is... | ...you automatically get this role on every project the org owns |
| :--- | :--- |
| Owner | Project **Owner** — full control, including destructive actions like deleting the project |
| Admin | Project **Maintainer** — can edit the project, manage runs, manage test cases, and manage collaborators, but cannot delete the project or transfer it out of the org |
| Member | Project **Viewer** — can see the project, its runs, and its reports, but cannot make changes |

The rationale, briefly:

- **Owners** paid for the organization. They should never be locked out of something the org owns.
- **Admins** handle operations. They need broad write access but should not be able to walk off with a project or delete it on a bad day.
- **Members** are the default. They can see what the org is working on, but turning a new hire into an editor on every project on day one is rarely what you want. Writing access should be granted deliberately.

This cascade is a fixed policy — it cannot be reconfigured per organization. A consistent cascade across every org is easier to explain, easier to audit, and easier to support than a bespoke permission matrix for each customer. If there is a concrete permissions need the cascade does not cover, the right answer is to grant that person a direct role on the specific project.

## Stacking: Cascades and Direct Grants

The cascade is the baseline, not the ceiling. A project member can also receive a **direct** project role — for example, when an Owner or Admin adds them to a specific project as an **Editor**.

When both a cascade and a direct grant apply, Muggle Test picks the **higher** of the two. Direct grants never downgrade a user.

Three quick examples:

1. **Promoting a Member on one project.** Priya is a Member of the org (cascade: Viewer). She is the lead on the checkout rewrite, so an Admin gives her a direct **Editor** role on the `checkout-flow-tests` project. Her effective role on that one project becomes **Editor**. Her role on every other org project stays Viewer via the cascade.

2. **Redundant grants are harmless.** Diego is an Admin of the org (cascade: Maintainer). Someone also adds him as a direct **Editor** on a specific project. His effective role on that project is still **Maintainer**, because the cascade is higher. The direct grant is effectively a no-op, but it does not break anything.

3. **Team grants count too.** Alex is a Member (cascade: Viewer). He is part of a Team that has been given **Editor** access to the `landing-page-tests` project. His effective role on that project is **Editor**, from the team grant.

The same "highest wins" rule applies no matter how many sources contribute. A user with a cascade of Viewer, a team grant of Editor, and a direct grant of Maintainer ends up as **Maintainer**.

## Where Access Can Come From

There are four places a role on an org project can come from. This matters because the [effective role lookup](#looking-up-someones-effective-role) below returns each of them separately.

| Source | What it means |
| :--- | :--- |
| **Direct** | Someone explicitly added this user to this project with a specific role. |
| **Team** | The user belongs to a team, and the team has been granted a role on this project. |
| **Owner** | The user created the project, so they are its implicit owner. |
| **Organization** | The project belongs to an organization and this user is a member of that organization. The role here is determined by the cascade described above. |

A user's **effective role** on a project is the highest role across however many of these sources apply. If none of them apply, the user has no access to the project.

## Looking Up Someone's Effective Role

Organization Owners and Admins can look up any member's effective role on any project the organization owns, along with the full breakdown of which sources contributed.

You will typically reach this from two places:

- **Organization Settings → Members → (pick a member) → Access on this org's projects.** Useful when you are reviewing a specific person's access — for example, during a quarterly access review, or when someone has just changed teams internally.
- **Project Settings → Members → (pick a member) → Why does this person have this role?** Useful when you are auditing a specific project and want to confirm nobody has more access than they should.

Either entry point returns the same thing:

- **Effective role.** The single role that is actually enforced when this person tries to do something on this project.
- **Contributing sources.** A list of every source (direct, team, owner, organization) that granted this person a role, and the role each one granted. If the user has no access, the list is empty.

### When to use the effective-role view

| You are trying to... | ...and the effective-role view helps because it shows |
| :--- | :--- |
| Understand why a new hire already has Viewer access to every project | The **organization** source granting Viewer via the cascade |
| Confirm a contractor only has access to the one project they should | The **direct** grant on the expected project and no sources on any other |
| Work out why a team member can still edit a project after leaving a team | Remaining **direct** or **organization** sources that the team removal did not touch |
| Verify a project owner really is the user you expect | The **owner** source |
| Debug a "permission denied" report from a teammate | An **empty** list of sources, or a role lower than the action requires |

### Things to keep in mind

- **This is a read-only view.** It only shows you what is currently true. Changing access still happens through the normal Project Settings and Organization Settings screens.
- **The view is privileged.** Because it surfaces another person's access, only organization Admins and Owners can see it. Regular Members cannot look up each other's roles.
- **It cannot cross organizations.** An Admin in one org cannot use the lookup to probe projects owned by a different org, even if they happen to know the project's identifier. Projects outside the org are reported as not found.
- **It matches what the system actually enforces.** The effective role returned by the lookup is computed by the exact same rules that gate access when a user tries to run a workflow or edit a test case. There is no "debug-only" shortcut — if the lookup says someone is a Maintainer, they are a Maintainer everywhere.

## Creating a Project Inside an Organization

When you create a project from inside an organization context, you can associate the project with that organization so it inherits everything on this page: the org's billing, the org's quotas, and the cascade from organization role to project role.

To associate a project with an org at creation time:

- You must be an active member of the organization (Owner, Admin, or Member). Members who have not accepted their invitation yet, or who have been deactivated, cannot create projects under an organization they "belong to" on paper.
- You must have at least **Member** role on that organization. In other words, any active org member can create an org project — creation is not locked down to Owners and Admins by default. Organizations that want to restrict who creates projects can turn off member project creation in **Organization Settings**.

Once the project is created under an organization:

- **The ownership is permanent.** You cannot move a project between personal and org ownership, and you cannot move it from one organization to another. This is a deliberate restriction so that every bill and every audit log has an unambiguous owner over the life of the project. If you need to "move" a project, recreate it under the new owner.
- **The cascade starts immediately.** Every current member of the org gets their cascade role on the new project the moment it is created. Future members will get it the moment their invitation is accepted.
- **Billing goes to the org wallet.** Workflow runs against the project debit the organization's wallet, not the creator's personal wallet, and the project's use cases and test cases count against the organization's quotas. See [Enterprise Organizations → Personal vs. Org Billing](billing/enterprise-organizations.md) for the full picture.

## Related Pages

| Topic | Page |
| :--- | :--- |
| What an organization is, pricing, seats, invitations | [Enterprise Organizations](billing/enterprise-organizations.md) |
| What happens to access when billing fails or recovers | [Billing Suspension and Recovery](billing/billing-suspension-and-recovery.md) |
| Plan comparison and enterprise pricing at a glance | [Subscription Plans](billing/subscription-plans.md) |
| Enterprise-specific quotas | [Quotas and Limits](billing/quotas-and-limits.md) |
