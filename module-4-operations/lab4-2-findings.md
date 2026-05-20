# Lab 4.2 — PACE Handover and Post-Incident Report

## Objective

The objective of this lab was to practice support handover documentation using the PACE method and to write a structured Post-Incident Report (PIR) after a simulated MFA incident.

---

## Tools Used

- GitHub
- Markdown
- Git commits
- Browser

---

## Step 1 — PACE Handover

A PACE shift handover file was created to document the current ticket status, pending actions, context, and expectations for the incoming engineer.

The handover included:

- Priority tickets
- Actions pending
- Incident context
- Expectations for the next engineer
- Sign-off checklist

File created:

```text
module-4-operations/handovers/handover-2026-04-08-1800.md
```
Step 2 — Post-Incident Report

A Post-Incident Report was created for the simulated MFA bulk OTP failure incident.

The PIR included:

Incident summary
Timeline
Root cause
Impact assessment
Resolution steps
Prevention actions
Open items

File created:
```text
module-4-operations/pirs/PIR-2026-04-08-MFA-Bulk-Failure.md
```
## PACE Handover Learning

The PACE handover helped organize shift transition information clearly. It ensured that the incoming engineer could quickly understand which tickets were urgent, what actions were pending, what context was already known, and what outcomes were expected.

PACE handovers are useful in support operations because they reduce missed follow-ups, improve SLA tracking, and make ownership transfer smoother between engineers.

## PIR Learning

The PIR helped document what happened during the incident, when it happened, who was affected, what the suspected root cause was, how the issue was handled, and what actions should be taken to prevent similar incidents.

A good PIR is important because it creates a permanent incident record and helps teams improve processes after real customer-impacting issues.
## Difference Between Handover and PIR
| Item     | PACE Handover                            | PIR                                          |
| -------- | ---------------------------------------- | -------------------------------------------- |
| Purpose  | Transfer active work to another engineer | Document an incident after resolution        |
| Timing   | During shift change                      | After incident is resolved                   |
| Focus    | Pending actions and current context      | Root cause, timeline, impact, and prevention |
| Audience | Incoming support engineer                | Support team, leads, and operations team     |
| Status   | Active / ongoing work                    | Completed incident review                    |
