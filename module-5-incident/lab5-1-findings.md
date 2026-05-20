# Lab 5.1 — Kill the Gateway: P1 Incident Simulation

## Objective

The objective of this lab was to simulate a P1 gateway outage, detect the failure using Uptime Kuma, create and manage a GitHub incident ticket, investigate using monitoring tools, restore the service, and document the incident using PIR and PACE handover formats.

---

## Tools Used

- AWS EC2 Ubuntu VM
- Nginx
- Uptime Kuma
- Prometheus
- Grafana
- Node Exporter
- GitHub Issues
- GitHub Projects
- Markdown

---

## Scenario

The Lab Nginx Gateway was intentionally stopped to simulate a gateway outage.

Command used to trigger the incident:

```bash
sudo systemctl stop nginx
```
his caused the Uptime Kuma monitor for the Lab Nginx Server to go DOWN.

## Step 1 — Incident Detection

Uptime Kuma detected that the Lab Nginx Server was DOWN.

The monitor showed a connection refused error on port 80, confirming that the Nginx web service was not reachable.

![Uptime Kuma red alert](../screenshots/lab5-1-uptime-kuma-red-alert.png)

## Step 2 — P1 Ticket Creation

A P1 incident ticket was created in GitHub Issues to track the outage.

### Ticket title:
```
[P1] Lab Nginx Gateway Unreachable — All Users Offline
```
Labels used:
```
P1-Critical
product:ztna
status:in-progress
```
## Step 3 — Investigation

The Nginx service status was checked on the VM and showed that the service was stopped.

Prometheus was also checked to confirm that the VM-level monitoring target was still UP. This showed that the issue was not a full VM outage, but a service-level outage affecting Nginx.

![Prometheus investigation](../screenshots/lab5-1-prometheus-investigation.png)

## Step 4 — Incident Timeline

The Uptime Kuma incident timeline showed the service going DOWN and later recovering.
### Observed timeline:
| Time (UTC) | Status | Message                               |
| ---------- | ------ | ------------------------------------- |
| 15:44      | Down   | connect ECONNREFUSED 13.60.163.172:80 |
| 16:05      | Up     | 200 - OK                              |
Approximate outage duration: 21 minutes

![Uptime Kuma incident timeline](../screenshots/lab5-1-uptime-kuma-timeline.png)

## Step 5 — Resolution

The Nginx service was restarted using:
```
sudo systemctl start nginx
```
After restarting Nginx, Uptime Kuma detected recovery and the Lab Nginx Server monitor returned to UP/green.

![Uptime Kuma recovery green](../screenshots/lab5-1-uptime-kuma-recovery-green.png)

## Step 6 — Ticket Closure

A resolution summary and closure comment were added to the GitHub P1 ticket. The ticket was then moved to Closed status.

![GitHub P1 ticket full thread 1](../screenshots/lab5-1-ticket-full-thread.png)

![GitHub P1 ticket full thread 2](../screenshots/lab5-1-ticket-full-thread2.png)

![GitHub P1 ticket full thread 3](../screenshots/lab5-1-ticket-full-thread3.png)

![GitHub P1 ticket full thread 4](../screenshots/lab5-1-ticket-full-thread4.png)

## Step 7 — Post-Incident Report

A PIR was created to document the incident summary, timeline, root cause, impact, resolution, prevention actions, and lessons learned.

### PIR file:

module-5-incidents/pirs/PIR-2026-05-20-Nginx-Gateway-Outage.md

![PIR file](../screenshots/lab5-1-pir-file.png)

## Step 8 — PACE Handover

A PACE handover was created to simulate shift handover after the incident.

PACE handover file:
```text
module-5-incidents/handovers/handover-2026-05-20-p1-nginx-outage.md
```
![PACE handover file 1](../screenshots/lab5-1-pace-handover-file.png)

![PACE handover file 2](../screenshots/lab5-1-pace-handover-file2.png)

## Root Cause

The root cause was that the Nginx service was stopped on the VM. This caused the HTTP monitor in Uptime Kuma to fail with a connection refused error on port 80.

The VM itself remained reachable, and Prometheus/Grafana monitoring continued to run, confirming that this was a service-level outage rather than a full infrastructure outage.

## Impact
Severity: P1
Affected service: Lab Nginx Gateway
Affected users: All users depending on the gateway
User impact: Gateway web service was unreachable
Outage duration: Approximately 21 minutes

## Resolution Summary

The Nginx service was restarted using sudo systemctl start nginx. After restart, Uptime Kuma detected recovery and the monitor returned to UP with a 200 - OK response.

### Prevention Actions

Add service-level monitoring for Nginx.
Add alerting for Nginx process/service state.
Create a runbook for Nginx restart and validation.
Add checks to distinguish service-level outages from VM-level outages.
Document escalation steps for P1 gateway incidents.
