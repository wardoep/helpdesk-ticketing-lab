# Ticket #100301 — New hire starts Monday (onboarding)

> **Synthetic example** authored to demonstrate the workflow. Not real user data.

| Field | Value |
|---|---|
| Requester | Sofia Grant (HR Manager) |
| Channel | Email → auto-created ticket |
| Help Topic | Account / Access → Onboarding |
| Department | IT Support |
| Priority | Normal (has lead time) |
| SLA | Tier-1 (respond 1h / resolve by start date) |
| Status | Closed |

## Thread

**Requester:** "New Sales rep, Alex Kim, starts Monday. Needs the usual — account, email, laptop, access to the Sales share and CRM."

**Agent — internal note:** Standard onboarding. Follow the checklist so nothing's missed and access is granted by *group*, never by direct grant (keeps it auditable — AGDLP). Confirmed with HR: department Sales, manager, start date, standard Sales access profile.

**Agent actions (checklist):**
1. Created `akim` in `OU=Sales` via the provisioning script (forced password change at first logon).
2. Added `akim` to `GG-Sales` — that group's membership *is* the access (Sales share, CRM), so no per-resource grants.
3. Provisioned mailbox / license.
4. Imaged and named a laptop (`SALES-LT-11`), ran the new-machine checklist, joined it to the domain.
5. Prepared a welcome sheet: username, temp password (delivered to manager), how to change it, VPN setup, where the Sales share maps.

**Agent → Requester:**
> Hi Sofia — Alex Kim is set up: account `akim`, mailbox active, laptop `SALES-LT-11` ready, and Sales share + CRM access granted via the Sales group. Temp password and a getting-started sheet are with you for their first day. Anything else Alex needs, reply here.

**Requester:** "Perfect, thank you — all set for Monday."

## Resolution

Full onboarding completed ahead of the start date using the standard checklist: account, mailbox, licensed device, and role-based access via group membership (not direct grants). Credentials delivered out-of-band to the manager.

**Root cause category:** n/a (service request, not an incident).
**KB:** [New-hire onboarding checklist](../knowledge-base/kb-04-onboarding-checklist.md)
**Time to resolve:** ~90 min of work, completed 3 days before start.
