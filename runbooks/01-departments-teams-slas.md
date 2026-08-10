# 01 — Departments, teams, and SLAs

**Goal:** the routing structure (who owns what) and the promises (SLA plans) that make the desk measurable.

## Steps

1. **Create departments** (*Admin Panel → Agents → Departments*). For the lab:
   - **IT Support** (the default Tier-1 desk)
   - **HR Support** (for HR-specific requests)
   Set each department's manager and its default response/SLA.

2. **Create teams** (*Agents → Teams*) — cross-department groups you can assign a ticket to, e.g. a "Networking" team pulling agents from IT. Teams let you route by skill without reorganising departments.

3. **Create SLA plans** (*Admin Panel → Manage → SLA*). An SLA is the committed clock. Define a few:
   | Plan | Grace / target | Use |
   |---|---|---|
   | Tier-1 Normal | resolve 8h (business) | routine requests |
   | Tier-1 High | resolve 4h | user fully blocked |
   | Major Incident | respond 15m, update 30m | multi-user / core service |
   Set what happens on breach (escalate, alert) and the business hours the clock respects.

4. **Set priorities** and map them so higher priority attaches a tighter SLA automatically.

## Verify

- Open a test ticket, assign it to **IT Support** with **High** priority, and confirm it picks up the **Tier-1 High** SLA and shows a due time.
- Reassign to a team and confirm visibility for that team's agents.

## If it breaks

- **Tickets show no due date.** No SLA is attached — either the department has no default SLA or the help topic (milestone 2) isn't setting one. Attach an SLA at the department or help-topic level.
- **Everything lands in the default department.** Routing is set by help topics (next milestone) — until those exist, intake falls back to the default.
