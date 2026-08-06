# Helpdesk Ticketing Lab

A working IT helpdesk built on **osTicket** in Docker, configured like a small company's support desk — departments, SLAs, help topics, canned responses, and email-to-ticket — with a set of worked example tickets and a matching knowledge base. Documented the same way I'd document a support operation I was handed.

## Overview

Helpdesk and desktop-support roles run on a ticketing system, and the skill that gets you hired isn't "I clicked around a ticket tool" — it's *understanding how a support desk is structured*: how tickets get routed to the right team, what an SLA actually commits you to, why canned responses and a knowledge base exist, and what the lifecycle of a ticket looks like from open to closed. This lab stands up a real osTicket instance to learn that structure hands-on, then documents five representative tickets end-to-end, each paired with a knowledge-base article a future agent could reuse.

The runbooks are the *how*; **[TAKEAWAYS.md](TAKEAWAYS.md) is the *why*** — how a support desk is organised and why each piece of that structure exists.

> The tickets in [`tickets/`](tickets/) are **synthetic examples** I authored to demonstrate the workflow — they are not real user data.

## Architecture

```
   User email ──IMAP fetch──┐
                            ▼
   Browser ──────────► osTicket (PHP/Apache container)
                            │
                            ▼
                     MariaDB container  (ticket + KB data on a named volume)
```

## Milestones

| # | Runbook |
|---|---------|
| 0 | [Deploy osTicket in Docker](runbooks/00-deploy-osticket.md) |
| 1 | [Departments, teams, and SLAs](runbooks/01-departments-teams-slas.md) |
| 2 | [Help topics and canned responses](runbooks/02-help-topics-and-canned.md) |
| 3 | [Email piping (email → ticket)](runbooks/03-email-piping.md) |
| 4 | [The agent workflow: life of a ticket](runbooks/04-agent-workflow.md) |
| 5 | [Sample tickets + knowledge base](runbooks/05-sample-tickets-and-kb.md) |

## What's in this repo

- [`compose/`](compose/) — `docker-compose.yml` for osTicket + MariaDB, and `.env.example`.
- [`tickets/`](tickets/) — five synthetic worked tickets (full thread + resolution): password reset, printer offline, VPN can't connect, new-hire onboarding, email outage.
- [`knowledge-base/`](knowledge-base/) — a reusable KB article matching each ticket.

## Skills this lab exercises

Ticketing-system administration (osTicket: departments, teams, agents, roles), SLA plans and priorities, ticket routing via help topics, canned responses and knowledge-base authoring, email-to-ticket piping over IMAP, the full ticket lifecycle (assign → work → internal notes vs replies → resolve → close), and the customer-service discipline of clear, reusable written resolutions — all deployed in Docker.

## What I learned

Filled in per milestone as the lab progresses — see the closing section of each runbook. The goal isn't a screenshot of a ticket tool; it's evidence I understand how a support desk is organised and can work a ticket from report to reusable knowledge.

---
Built and maintained by **Edward J. Penna** — [github.com/wardoep](https://github.com/wardoep)
