# 05 — Sample tickets + knowledge base

**Goal:** work each of the five representative scenarios through the live system, and publish the matching knowledge-base articles — turning solved problems into reusable knowledge.

The written-up versions live in [`tickets/`](../tickets/) and [`knowledge-base/`](../knowledge-base/); this runbook is about *enacting* them in osTicket.

## Steps

1. **Load the KB articles into osTicket.** In *Knowledgebase → Categories*, create categories (Accounts, Hardware, Network, Email), then add an FAQ article per KB file. Mark the reusable ones **public** so users can self-serve; keep internal-only ones **internal**.

2. **Work each sample scenario as a real ticket** (using the workflow from [runbook 04](04-agent-workflow.md)):
   | Scenario | Shows |
   |---|---|
   | [Password reset / lockout](../tickets/01-password-reset.md) | identity verification; self-inflicted lockout vs attack |
   | [Printer offline](../tickets/02-printer-offline.md) | "many users, one device" → server/queue |
   | [VPN can't connect](../tickets/03-vpn-cannot-connect.md) | reading the exact error to narrow cause |
   | [Onboarding](../tickets/04-new-hire-onboarding.md) | service request from a checklist; access by group |
   | [Email outage](../tickets/05-email-outage.md) | major-incident handling + preventive fix |

3. **Link each ticket to its KB article** so the resolution points at the reusable answer.

4. **Review the reporting** (*Dashboard*) — ticket volume by topic, SLA performance — the numbers a support lead actually watches.

## Verify

- All five scenarios exist as worked tickets in `/scp`, each Resolved/Closed with a clear summary and a linked KB article.
- The public KB articles appear in the user portal and are searchable.

## If it breaks

- **KB article won't show to users.** It's marked internal or its category is internal — set both to public.
- **Can't link a ticket to a KB article.** Reference it in the resolution note / reply; osTicket links FAQs to help topics rather than directly to tickets.
