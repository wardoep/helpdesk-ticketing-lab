# 03 — Email piping (email → ticket)

**Goal:** a support mailbox whose incoming mail automatically becomes tickets, so users can just email IT and nothing dies in an inbox.

## How osTicket fetches mail

osTicket supports **IMAP/POP fetching**: on a schedule it logs into the support mailbox, pulls new messages, and turns each into a ticket (replies to an existing ticket thread if the subject carries the ticket number). This is the "pull" model — simpler than server-side piping and works with any mailbox you can reach over IMAP.

## Steps

1. **Provision a support mailbox** — e.g. `support@corp.lab` (or a test Gmail/lab mail account). Note the IMAP host, port (993/SSL), and an app password if the provider requires one.

2. **Add the email in osTicket** (*Admin Panel → Emails → Add New Email*):
   - The address and display name (this becomes a system email).
   - **Mail fetching:** enable IMAP, enter host/port/SSL, username, password/app-password.
   - **New-ticket settings:** the department and help topic to assign fetched mail to, and whether to auto-respond.

3. **Enable the cron/fetch schedule.** osTicket fetches on its cron. In the container, ensure the scheduler runs (the official image runs it; confirm with the *Dashboard → Information* "last cron" time advancing).

4. **Set the autoresponder** so a user emailing in immediately gets "Ticket #NNN received" with the number — that number is what threads their replies.

## Verify

1. Send an email from a normal account to the support mailbox.
2. Within a fetch cycle, a new ticket appears in `/scp` under the configured department/topic.
3. Reply from the agent panel; confirm the user receives it and that the user's reply lands back on the **same** ticket (not a new one).

## If it breaks

- **No tickets appear.** Fetching isn't running or credentials are wrong — check *Dashboard → Information* for the last-fetch time and any mail errors; test the IMAP login separately.
- **Every reply opens a NEW ticket.** The ticket number/threading token isn't surviving the round trip — confirm the outbound email includes the ticket ID and the fetcher is matching it.
- **Gmail/Microsoft rejects the login.** Modern providers need an app password or OAuth, not the account password; use a provider that allows IMAP app passwords for the lab.

## What I learned

*Filled in after I complete this milestone.*

<!-- Prompts to answer once done:
     - Pull (IMAP fetch) vs push (server piping) — why did I use fetch and what's the trade-off?
     - How does threading actually work — what makes a reply attach to the right ticket?
     - What did the autoresponder need to contain for threading to survive? -->
