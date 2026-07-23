# KB-04 — New-hire onboarding checklist

**Audience:** IT Support agents · **Applies to:** any new employee

Confirm with HR before starting: **full name, department, manager, start date, and the standard access profile for the role.**

## Checklist

- [ ] **Account** — create in the correct department OU; force password change at first logon. Use the provisioning script, not manual clicks, so the username follows convention.
- [ ] **Group membership (access)** — add to the department group(s). Access is granted **by group, never by direct grant on a resource** — this keeps it auditable and makes future role changes one edit (AGDLP model).
- [ ] **Mailbox / license** — provision and license email and any SaaS the role needs.
- [ ] **Device** — image, name per convention, run the new-machine checklist (name/timezone/DNS/clock), join to the domain.
- [ ] **Role apps** — CRM, line-of-business tools, shared drives (via the group, above).
- [ ] **Credential delivery** — temp password delivered **out-of-band to the manager**, never emailed to a mailbox the user can't yet reach.
- [ ] **Welcome sheet** — username, how to change the password, VPN setup, where shares map, who to contact for help.
- [ ] **Verify before start date** — log in as the account (or have the manager) to confirm access actually works.

## Why by-group, not direct

Granting a user directly onto a share works once and then rots into something unauditable. Group-based access means "what can this person reach?" is answerable by reading group membership, and a department change is a single group swap. See the AGDLP model in [ad-network-lab](https://github.com/wardoep/ad-network-lab).

## Pair with offboarding

Everything granted here should be reversible the same way — see the offboarding tool in [powershell-admin-toolkit](https://github.com/wardoep/powershell-admin-toolkit).
