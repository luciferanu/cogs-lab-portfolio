# Lab 1.2 Findings

## Lab Title

Nginx Web Server and Self-Signed TLS Certificate Setup

## My VM Details

- Provider: AWS
- Region: eu-north-1
- OS: Ubuntu 26.04 LTS
- Public IPv4: 13.60.163.172
- VM User: ubuntu

---

## Experiment 1: Nginx HTTP Test

![HTTP Test Screenshot](../screenshots/lab1-2-http-test.png)

## Experiment 2: Self-Signed Certificate Creation

![Certificate Creation Screenshot](../screenshots/lab1-2-cert-created.png)

## Experiment 3: Nginx HTTPS Configuration Test

![Nginx Config Test Screenshot](../screenshots/lab1-2-nginx-config-test.png)

## Experiment 4: HTTPS Test

![HTTPS Test Screenshot](../screenshots/lab1-2-https-test.png)

## Experiment 5: TLS Certificate Inspection

![Certificate Inspection Screenshot](../screenshots/lab1-2-cert-inspection.png)

## Explanation: Why curl without -k fails

When I use HTTPS with a self-signed TLS certificate, the connection is encrypted, but the certificate is not trusted by default.

A normal trusted HTTPS certificate is issued by a trusted Certificate Authority. In this lab, I created the certificate myself using OpenSSL, so it is self-signed. Because of this, curl cannot verify that the certificate came from a trusted authority.

## Testing HTTPS Access

The HTTPS endpoint was tested using curl with the `-k` option:

```bash
curl -k https://13.60.163.172
```

The `-k` option tells curl to ignore certificate trust validation errors because the lab used a self-signed certificate.

Observed result:

- HTTPS connection successful
- Nginx page loaded correctly
- TLS encryption active

---

## Certificate Inspection

The certificate was inspected using OpenSSL:

```bash
openssl s_client -connect 13.60.163.172:443
```

Important certificate findings:

| Field | Observation |
|---|---|
| Certificate Type | Self-signed |
| TLS Version | TLSv1.2 / TLSv1.3 supported |
| Port | 443 |
| Issuer | Self-generated lab certificate 
| Certificate Expiry | 365 days |
| Purpose | HTTPS encryption testing |

The certificate inspection confirmed that the HTTPS service was active and the Nginx server was successfully presenting a TLS certificate.

The OpenSSL inspection output also confirmed that the certificate chain was self-signed, which is expected in a lab environment.
