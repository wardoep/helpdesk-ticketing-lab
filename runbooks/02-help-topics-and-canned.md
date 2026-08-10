# 02 — Help topics and canned responses

**Goal:** intake that routes and prioritises itself, plus reusable replies for the repeatable parts.

## Steps

1. **Create help topics** (*Admin Panel → Manage → Help Topics*). Each topic the user picks routes the ticket to a department and can set priority and SLA automatically:
   | Help topic | Routes to | Priority |
   |---|---|---|
   | Account / Access → Password & Lockout | IT Support | High |
   | Network → Remote Access / VPN | IT Support | High |
   | Hardware → Printing | IT Support | Normal |
   | Account / Access → Onboarding | IT Support | Normal |
   | Email → Outage | IT Support | Emergency |
   This is the system doing triage: the user's own selection sets owner + urgency.

2. **Add ticket priorities** if the defaults don't fit, and confirm the emergency/critical one maps to the Major-Incident SLA.

3. **Create canned responses** (*Admin Panel → Manage → Canned Responses*) for the replies you send constantly — e.g. "Password reset — temporary issued, change at first login," "We're aware of the email delay (major incident update)." Use variables (`%{ticket.name.first}`) so they personalise.

4. **Wire topics to forms** if you want a topic to ask for specific info up front (e.g. the VPN topic asking for the client version and error text) — good intake reduces back-and-forth.

## Verify

- Submit a test ticket as a user, choosing **Password & Lockout**; confirm it lands in **IT Support** at **High** with the right SLA — without an agent touching it.
- In an agent reply, insert a canned response and confirm the variables fill in.

## If it breaks

- **A topic routes to the wrong department.** Check the topic's *Department* and *Priority* overrides — the topic wins over the department default.
- **Canned variables show literally (`%{...}`).** Wrong variable name; use the picker in the canned-response editor.
