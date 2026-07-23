# Ticket #100288 — VPN won't connect from home

> **Synthetic example** authored to demonstrate the workflow. Not real user data.

| Field | Value |
|---|---|
| Requester | Priya Nair (Sales, remote) |
| Channel | Email → auto-created ticket |
| Help Topic | Network → Remote Access / VPN |
| Department | IT Support |
| Priority | High (remote user fully blocked) |
| SLA | Tier-1 (respond 1h / resolve 4h) |
| Status | Closed |

## Thread

**Requester:** "VPN client just spins and then says 'authentication failed.' Worked last week. I'm remote all day today."

**Agent — internal note:** "Authentication failed" (not "server unreachable") narrows it to credentials/MFA/account state, not connectivity. Checked directory: `pnair` enabled, not locked. Password *last set* 91 days ago — domain policy max age is 90. High-probability cause: expired password that the VPN can't prompt to change.

**Agent → Requester:**
> Hi Priya — your VPN "authentication failed" because your domain password expired yesterday, and the VPN client can't walk you through the change screen the way a normal login does. I've reset it to a temporary password (sent via your manager per policy); sign in to the webmail portal first to set a new permanent password, then reconnect the VPN with the new one. That order matters — the portal lets you change it, the VPN doesn't.

**Agent actions:** confirmed password age; reset with forced change; documented the reset in the ticket and to the user's manager (out-of-band delivery of temp credential).

**Requester:** "Changed it on the portal, VPN connected. Thank you!"

## Resolution

Password had passed the 90-day max-age while the user was remote; the VPN client couldn't surface the change prompt, presenting as "authentication failed." Reset with forced change, directed the user to change it via the web portal *first*, then reconnect. Closed after confirmation.

**Root cause category:** expired password + client that can't prompt for change.
**KB:** [VPN "authentication failed" — the expired-password trap](../knowledge-base/kb-03-vpn-authentication-failed.md)
**Time to resolve:** ~25 min.
