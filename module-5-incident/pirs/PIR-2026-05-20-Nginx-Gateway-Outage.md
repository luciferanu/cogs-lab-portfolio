# Post-Incident Report — Nginx Gateway Outage

**Incident ID:** INC-2026-05-20-001  
**Date:** 2026-05-20  
**Severity:** P1  
**Status:** Resolved  
**Service:** Lab Nginx Gateway  

---

## Incident Summary

On 2026-05-20, Uptime Kuma detected that the Lab Nginx Server monitor was DOWN. The incident was treated as a P1 because the lab gateway service was unreachable for all users. Investigation showed that the VM was still reachable and Prometheus/Grafana monitoring remained available, but the Nginx service was stopped. The service was restored by starting Nginx again.

---

## Detection

The incident was detected by Uptime Kuma when the Lab Nginx Server monitor changed from UP to DOWN.

Detection source:

```text
Uptime Kuma red alert
```
## Timeline
| Time (UTC) | Event                                                    |
| ---------- | -------------------------------------------------------- |
| 15:44      | Uptime Kuma detected Lab Nginx Server DOWN               |
| 15:45      | P1 GitHub incident ticket created                        |
| 15:46      | Nginx service status checked on VM                       |
| 15:48      | Prometheus target checked and VM monitoring confirmed UP |
| 16:03      | Nginx service restarted                                  |
| 16:05      | Uptime Kuma detected recovery and monitor returned UP    |
| 16:06      | Resolution summary added to ticket                       |
| 16:08      | Incident ticket closed                                   |

### Root Cause

The root cause was that the Nginx service was stopped on the VM. This caused the Lab Nginx Server HTTP monitor in Uptime Kuma to fail with a connection refused error on port 80. The VM itself was still reachable, and Prometheus/Grafana monitoring containers were still running.
## Impact
Severity: P1
Affected service: Lab Nginx Gateway
Affected users: All users depending on the lab gateway
User impact: Gateway web service was unreachable
Monitoring impact: Uptime Kuma showed Nginx monitor DOWN
Error observed: connect ECONNREFUSED 13.60.163.172:80
Recovery observed: 200 - OK
Approximate outage duration: 21 minutes

### Investigation Notes

During investigation:

Uptime Kuma showed the Lab Nginx Server monitor as DOWN.
Uptime Kuma timeline showed connect ECONNREFUSED 13.60.163.172:80.
systemctl status nginx showed Nginx was inactive/stopped.
Prometheus, Grafana, and Node Exporter containers were still running.
Prometheus target status confirmed VM-level monitoring was still available.
This showed that the incident was service-level, not VM-level.

### Resolution

The Nginx service was started again using:
```
sudo systemctl start nginx
```
After restarting Nginx, Uptime Kuma detected recovery and the Lab Nginx Server monitor returned to UP/green with 200 - OK.

### Prevention Actions
 Add service-level monitoring for Nginx.
 Add alerting for Nginx process/service state.
 Create a runbook for restarting and validating Nginx.
 Add a check to confirm whether outage is service-level or VM-level.
 Document escalation steps for P1 gateway outage.

### Lessons Learned

This incident showed the importance of monitoring both infrastructure and application services. Prometheus confirmed the VM was still healthy, while Uptime Kuma detected that the Nginx service itself was unavailable. This helped identify that the outage was limited to the gateway service rather than the full VM.
