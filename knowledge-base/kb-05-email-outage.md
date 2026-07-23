# KB-05 — Handling an email / mail-flow outage (major incident)

**Audience:** IT Support agents · **Applies to:** office-wide email disruption

## Recognise it's a major incident

Multiple users + a core service = **major incident**. Don't work it as scattered individual tickets — open **one master ticket**, link duplicates to it, and communicate on a schedule.

## Scope it fast

Narrow the failure before touching anything:
- **Send, receive, or both?** Can users open **webmail**? If webmail works but new mail isn't arriving → **inbound mail flow**, not the clients.
- **Everyone, or one department/site?** All = server/gateway; one site = network path.

## Common causes (check in order)

1. **Mail store out of disk** — a full mail-store disk stops accepting deliveries; mail piles up in the queue. Free space, watch the queue drain. (Very common, very preventable.)
2. **Mail queue stuck / service down** — restart the transport/queue service; look for a poison message.
3. **DNS / connectivity to the mail provider** — if mail is cloud-relayed, check the connector and DNS.
4. **Expired TLS cert on the mail connector** — deliveries fail TLS.

## Communicate (every 30 min until resolved)

> "We're aware email is delayed office-wide and working it. Webmail is reachable; queued mail will deliver once resolved. Next update in 30 minutes."

## Close the loop

1. Verify **send and receive** with a test message.
2. Confirm the queue has fully drained.
3. Post an all-clear with a one-line cause.
4. **Add a preventive control** — e.g. an alert on mail-store disk < 15% so capacity issues are caught before they're outages.

## The lesson

Most "email is down" incidents are capacity or a stuck queue, not anything exotic. Scope first, communicate on a cadence, and leave behind a monitor so the same cause can't surprise you twice.
