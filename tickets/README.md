# tickets/

Five **synthetic** worked tickets, each demonstrating a common support scenario end-to-end — the report, the agent's diagnostic reasoning (as internal notes), the user-facing replies, the actions taken, and the resolution. Each links to a reusable knowledge-base article.

These are illustrative examples authored for this lab, **not real user data**.

| # | Ticket | Type | KB article |
|---|---|---|---|
| 01 | [Password reset / lockout](01-password-reset.md) | Incident | [kb-01](../knowledge-base/kb-01-password-reset-and-lockout.md) |
| 02 | [Printer offline](02-printer-offline.md) | Incident | [kb-02](../knowledge-base/kb-02-printer-offline.md) |
| 03 | [VPN can't connect](03-vpn-cannot-connect.md) | Incident | [kb-03](../knowledge-base/kb-03-vpn-authentication-failed.md) |
| 04 | [New-hire onboarding](04-new-hire-onboarding.md) | Service request | [kb-04](../knowledge-base/kb-04-onboarding-checklist.md) |
| 05 | [Email outage](05-email-outage.md) | Major incident | [kb-05](../knowledge-base/kb-05-email-outage.md) |

## What each is meant to show

- **01** — identity verification before any account change; distinguishing a self-inflicted lockout from an attack.
- **02** — "many users, one device" points at the server/queue, not a single PC.
- **03** — reading the *exact* error ("authentication failed" ≠ "unreachable") to narrow the cause.
- **04** — a service request worked from a checklist; access granted by group (AGDLP), not directly.
- **05** — escalating to major-incident handling: one master ticket, scoping send vs receive, regular status updates, and a preventive fix.
