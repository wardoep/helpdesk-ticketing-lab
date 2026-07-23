# Ticket #100251 — Shared printer shows "Offline," can't print

> **Synthetic example** authored to demonstrate the workflow. Not real user data.

| Field | Value |
|---|---|
| Requester | Marcus Bell (HR) |
| Channel | Email → auto-created ticket |
| Help Topic | Hardware → Printing |
| Department | IT Support |
| Priority | Normal |
| SLA | Tier-1 (respond 1h / resolve 8h) |
| Status | Closed |

## Thread

**Requester (email):** "The HR printer (HR-MFP-2) says Offline. Nobody on our floor can print. It was fine yesterday."

**Agent — internal note:** Multiple users affected on one device = server/queue or network side, not one PC. Checked the print server: the queue for `HR-MFP-2` had a stuck job at the head, status "Error – Printing." Printer itself powered on, reachable by ping. Pattern = a jammed job wedging the queue.

**Agent → Requester:**
> Hi Marcus — a stuck print job was jamming the whole HR queue. I cleared it and restarted the print spooler; HR-MFP-2 is back online. Please try the print that failed. If it errors again I'll check the driver on your PC specifically.

**Agent actions:**
1. On the print server: cleared the errored job, `Restart-Service Spooler`.
2. Confirmed the printer's port/IP still matched (no DHCP change).
3. Sent a test page from the server.

**Requester:** "Printing again, thanks."

## Resolution

A single errored job stalled the shared queue, making the printer appear offline to all HR users. Cleared the queue, restarted the spooler on the print server, verified with a test page. Advised the user to report immediately if it recurs (would point to a driver or the device itself).

**Root cause category:** print queue stalled by a bad job.
**KB:** [Fixing a shared printer stuck "Offline"](../knowledge-base/kb-02-printer-offline.md)
**Time to resolve:** ~20 min.
