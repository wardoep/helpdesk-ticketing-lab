# Ticket #100234 — Can't log in, "account locked"

> **Synthetic example** authored to demonstrate the workflow. Not real user data.

| Field | Value |
|---|---|
| Requester | Dana Ruiz (Sales) |
| Channel | Phone → agent created ticket |
| Help Topic | Account / Access → Password & Lockout |
| Department | IT Support |
| Priority | High (user fully blocked) |
| SLA | Tier-1 (respond 1h / resolve 4h) |
| Status | Closed |

## Thread

**Requester (paraphrased by agent):** "I keep getting 'your account is locked.' I didn't change anything. I need into my email for a customer call in an hour."

**Agent — internal note:** Verified identity (asked for employee ID + manager name, matched directory). On DC01, `Search-ADAccount -LockedOut` shows `druiz` locked. Event 4740 source workstation = `SALES-LT-07`. Likely a saved/stale credential on a device re-trying an old password (classic lockout cause), not a live attack — no burst of 4625s from outside.

**Agent → Requester (reply):**
> Hi Dana — I've unlocked your account and set a temporary password; you'll be asked to change it at first sign-in. The lockout came from an old saved password on one of your devices. When you sign in, check the Wi-Fi/email password saved on your phone — that's the usual culprit. Let me know if it locks again.

**Agent action:** `Unlock-ADAccount druiz`; `Set-ADAccountPassword -Reset`; `Set-ADUser druiz -ChangePasswordAtLogon $true`.

**Requester:** "In now, thank you — updated the password on my phone too."

## Resolution

Account was locked by a stale saved credential on the user's phone. Unlocked, issued a temp password with forced change, and directed the user to update the saved password on their mobile device. Closed after confirmation.

**Root cause category:** self-inflicted lockout (stale saved credential).
**KB:** [How to reset a password and clear an account lockout](../knowledge-base/kb-01-password-reset-and-lockout.md)
**Time to resolve:** ~15 min (well within SLA).
