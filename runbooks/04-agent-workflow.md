# 04 — The agent workflow: life of a ticket

**Goal:** fluency in working a ticket start to finish, and the one distinction that matters most — internal notes vs public replies.

## The lifecycle

```
Open ──assign──► In progress ──(work + notes)──► Resolved ──confirm──► Closed
                     │
                     └── waiting on user (paused clock) ◄──►
```

## Steps (do this with one of the sample scenarios)

1. **Triage & assign.** Open a ticket, confirm the help topic routed it correctly, assign it to yourself or a team, set/verify priority and SLA.

2. **Work it, and record reasoning as INTERNAL NOTES.** Post your diagnostic steps as **internal notes** — these are *not* sent to the user. This is where "checked event 4740, source is SALES-LT-07, looks like a stale credential" goes.

3. **Communicate with the user as REPLIES.** A **reply** is emailed to the user. Keep it clear, blame-free, and actionable. **Never** put internal reasoning or candid remarks in a reply — that's the classic, career-limiting mistake.

4. **Use "waiting on user" to pause the SLA clock** when the ball is in their court, so you're not penalised for their response time.

5. **Resolve, then close.** Mark **Resolved** with a clear resolution summary; **Close** after the user confirms (or after the auto-close window). A ticket isn't closed until it's actually fixed *and* documented.

6. **Spin off a KB article** if the resolution is reusable — link it on the ticket.

## Verify

Work [sample ticket #100234](../tickets/01-password-reset.md) through the real system: create it, add an internal note with your diagnosis, send a public reply, resolve with a summary, and close. Confirm the internal note never appeared in the user's email.

## If it breaks

- **An internal note went to the user.** You used *Reply* instead of *Post Internal Note* — they're different buttons for a reason. Slow down and check which you're in.
- **SLA breached while waiting on the user.** You didn't set the status to "waiting on user" to pause the clock.
- **Ticket closed but the user comes back.** Resolution wasn't verified — prefer resolve-then-confirm over closing immediately.
