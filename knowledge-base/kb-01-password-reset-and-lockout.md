# KB-01 — Reset a password and clear an account lockout

**Audience:** IT Support agents · **Applies to:** Active Directory accounts

## Before you touch the account: verify identity

Never reset a password on request alone. Confirm the requester is who they say (employee ID + a second factor your org uses — manager name, callback to a listed number). A password reset for the wrong person is a security incident.

## Clear a lockout

1. Find the lock: `Search-ADAccount -LockedOut` (or ADUC → the account → *Unlock*).
2. Unlock: `Unlock-ADAccount <user>`.
3. **Find the source** so it doesn't re-lock: Security event **4740** on the DC names the workstation that caused it. A stale saved credential (phone Wi-Fi/email, a mapped drive, a service using old creds) is the usual culprit.

## Reset a password

```powershell
Set-ADAccountPassword -Identity <user> -Reset
Set-ADUser -Identity <user> -ChangePasswordAtLogon $true
```
Deliver the temporary password **out-of-band** (not in the same email thread) and require a change at first logon.

## Tell the user how to prevent a repeat

If the lockout came from a saved credential, the fix isn't complete until the user updates the saved password on the offending device (commonly a phone). Say so explicitly.

## Escalate if

- The 4740 source is external/unknown or there's a burst of failed logons (4625) → possible attack, involve security.
- The account keeps re-locking after the saved-credential fix → hunt for a service or scheduled task using old credentials.
