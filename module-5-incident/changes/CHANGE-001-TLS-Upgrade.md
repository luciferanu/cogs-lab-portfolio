# CHANGE-001 — TLS Upgrade Change Plan

## Change Details

**Change ID:** CHANGE-001  
**Type:** Normal Change  
**Status:** Implemented
**Service:** Nginx TLS Configuration  
**Environment:** Lab EC2 Ubuntu VM  

---

## Description

This change upgrades the Nginx TLS configuration from TLS 1.2-only to TLS 1.3-preferred. The purpose of the change is to improve security posture and align the lab gateway with modern TLS best practices.

---

## Justification

TLS 1.3 provides stronger security and improved performance compared to older TLS versions. Updating the Nginx configuration to prefer TLS 1.3 helps reduce reliance on older protocol behavior and supports safer encrypted communication.

---

## Execution Plan

1. Backup the current Nginx TLS configuration.
2. Update the `ssl_protocols` line in the Nginx site configuration.
3. Test the Nginx configuration using `nginx -t`.
4. Reload Nginx if the test is successful.
5. Verify TLS 1.3 using `openssl`.

---

## Rollback Plan

If TLS verification fails or the site becomes unreachable:

1. Restore the backup configuration.
2. Test the restored Nginx configuration using `nginx -t`.
3. Reload Nginx.
4. Confirm the site is reachable again.

Rollback command:

```bash
sudo cp /etc/nginx/sites-available/lab-tls.backup /etc/nginx/sites-available/lab-tls
sudo nginx -t
sudo systemctl reload nginx
```
### Risk Assessment

### Risk Level: Medium

### Possible risks:

Nginx configuration syntax error
HTTPS service disruption
TLS verification failure
Browser or client compatibility issue

### Maintenance Window

Window: Immediate lab window
Expected Duration: 10–15 minutes
Customer Impact: No real customer impact because this is a lab environment

## Approval

Approval Status: Approved and Implemented
This change plan must be committed before execution.

---

## Implementation Evidence

The Nginx configuration was updated and tested successfully.

Validation commands used:

```bash
sudo nginx -t
sudo systemctl reload nginx
openssl s_client -connect localhost:443 -tls1_3 2>/dev/null | grep 'Protocol'
```
Observed result:

Protocol: TLSv1.3

---

## Rollback Test Evidence

A failed change scenario was simulated by adding an invalid directive to the Nginx configuration.

Failure command used:

```bash
echo "bad_directive_here;" | sudo tee -a /etc/nginx/nginx.conf
sudo nginx -t
```
### Observed result:
```
nginx configuration test failed due to invalid directive
```
### Rollback command used:
```
sudo cp /etc/nginx/nginx.conf.before-failure-test /etc/nginx/nginx.conf
sudo nginx -t
sudo systemctl reload nginx
sudo systemctl status nginx --no-pager
```
### Observed rollback result:

nginx configuration syntax was OK, test was successful, and Nginx returned to active/running state.

