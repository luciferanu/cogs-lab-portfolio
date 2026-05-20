# Post-Incident Report (PIR)

**Incident ID:** INC-2026-04-08-001  
**Date:** 2026-04-08  
**Severity:** P2  
**Status:** Resolved  

---

## Incident Summary

On 2026-04-08, a mid-size financial customer reported that 25 finance department users were unable to receive SMS OTP during login. Username and password authentication was working, and email OTP was also working as an alternate MFA method. The issue was isolated to the SMS OTP delivery path. The ticket was escalated to L2 for SMS provider and sender ID verification. Users were able to complete MFA using the alternate OTP method while SMS delivery was being investigated.

---

## Timeline

| Time (IST) | Event |
|---|---|
| 09:15 | Customer reported MFA SMS OTP failure for 25 users |
| 09:18 | Ticket #1001 created with P2 priority |
| 09:20 | First response sent to customer |
| 09:35 | Phone numbers verified as correct |
| 10:00 | Username/password and email OTP confirmed working |
| 10:30 | Issue escalated to L2 |
| 11:30 | L2 identified suspected SMS sender ID or carrier-level rejection |
| 11:40 | Backup/email OTP method used as workaround |
| 11:45 | Customer confirmed users could complete MFA |

---

## Root Cause

The suspected root cause was SMS OTP delivery rejection at the SMS provider, sender ID, carrier, or DND level. The issue did not affect username/password authentication or email OTP. This indicates that the failure was specific to the SMS delivery channel rather than the complete MFA system.

---

## Impact Assessment

- Users affected: 25
- Customer segment: Financial institution
- Department affected: Finance
- Business impact: Users were temporarily unable to complete MFA using SMS OTP
- Duration: Approximately 2 hours 30 minutes
- Workaround: Email OTP / alternate OTP method

---

## Resolution Steps

1. Confirmed that username/password authentication was working.
2. Confirmed that email OTP was working.
3. Verified affected phone numbers were correct.
4. Identified that the issue was limited to SMS OTP delivery.
5. Escalated to L2 for SMS provider logs, sender ID status, and rejection code verification.
6. Used alternate/email OTP as a workaround.
7. Received customer confirmation that users were able to complete MFA.

---

## Prevention Actions

- [ ] Add monitoring for SMS OTP delivery failure rates.
- [ ] Track sender ID rejection patterns from the SMS provider.
- [ ] Create an internal KB article for SMS OTP delivery troubleshooting.
- [ ] Add a checklist for verifying alternate MFA methods during MFA incidents.
- [ ] Define escalation criteria for bulk OTP delivery failures.

---

## Open Items

- [ ] L2 to confirm final SMS provider rejection reason.
- [ ] Support team to document SMS OTP troubleshooting steps in the knowledge base.
- [ ] Review whether alerting can be added for sudden SMS OTP delivery failure spikes.
