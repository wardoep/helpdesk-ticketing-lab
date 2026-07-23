# KB-03 — VPN "authentication failed" (the expired-password trap)

**Audience:** IT Support agents · **Applies to:** domain-authenticated VPN

## Read the error precisely

- **"Authentication failed" / "wrong credentials"** → the tunnel reached the server; the problem is the **account or password**. This KB.
- **"Server unreachable" / timeout** → connectivity/DNS/firewall, a different path — check the client's internet, the VPN gateway address, and any local firewall.

## The most common cause: expired password

Many VPN clients authenticate but **cannot present the "your password expired, set a new one" screen** that a normal Windows login does. So an expired password shows up as a flat "authentication failed."

Check on the DC:
```powershell
Get-ADUser <user> -Properties PasswordLastSet, Enabled, LockedOut |
  Select-Object Enabled, LockedOut, PasswordLastSet
```
If `PasswordLastSet` is older than the domain's max password age, that's it.

## Fix (order matters)

1. Reset the password with a forced change; deliver the temp password out-of-band.
2. Have the user **change it via a portal that can prompt for a change first** (webmail/self-service), *then* connect the VPN with the new password.
3. Confirm the VPN connects.

## Also rule out

- Account locked or disabled (`LockedOut`, `Enabled`).
- MFA token drift or a new device not enrolled.
- The user typing the *old* password out of habit.

## Prevent

Notify users of pending password expiry by email a week ahead; remote-heavy users hit this most.
