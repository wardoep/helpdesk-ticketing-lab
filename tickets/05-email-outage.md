# Ticket #100317 — "Email is down for the whole office"

> **Synthetic example** authored to demonstrate the workflow. Not real user data.

| Field | Value |
|---|---|
| Requester | Multiple (first reported by reception) |
| Channel | Phone (email itself was down) |
| Help Topic | Email → Outage |
| Department | IT Support |
| Priority | Critical (multi-user, core service) |
| SLA | Major-incident (respond 15m / updates every 30m) |
| Status | Closed |

## Thread

**First report (phone):** "Nobody's getting email. Outlook says 'disconnected.'"

**Agent — internal note (triage):** Multi-user + core service → treat as a **major incident**, not a normal ticket. Opened this master ticket, will link duplicates to it rather than working them separately. Scope check: is it *send*, *receive*, or *both*? Reception can open webmail but new mail isn't arriving → inbound mail flow, not client-side. Checked the mail gateway/queue: inbound messages piling up in the queue, not being delivered. Disk on the mail store at 0% free — the store stopped accepting deliveries.

**Agent — status update (posted to all affected, every 30 min):**
> We're aware email delivery is delayed office-wide and actively working it. Webmail is reachable; new messages are queued and will deliver once resolved. Next update in 30 minutes.

**Agent actions:**
1. Freed space on the mail store (cleared expired logs/temp), bringing it back above threshold.
2. Watched the queue drain as deliveries resumed.
3. Confirmed send + receive with a test message.
4. Added a monitoring alert for mail-store disk < 15% so this is caught *before* it's an outage next time.

**Closing note to office:**
> Email is fully restored and the backlog has delivered. Cause was the mail server running out of disk space, which stopped incoming delivery. We've added an alert so we're warned well before it can happen again. Thanks for your patience.

## Resolution

Office-wide inbound mail delay caused by the mail store reaching 0% free disk, halting deliveries. Freed space, queue drained, verified send/receive, and added a proactive disk-space alert. Handled as a major incident with a linked master ticket and regular status updates.

**Root cause category:** capacity (disk full) on a core service.
**KB:** [Handling an email/mail-flow outage (major incident)](../knowledge-base/kb-05-email-outage.md)
**Time to resolve:** ~40 min from first report to restored.
