# Lab 1.2 Findings

## Lab Title

Nginx Web Server and Self-Signed TLS Certificate Setup

## My VM Details

- Provider: AWS
- Region: eu-north-1
- OS: Ubuntu 26.04 LTS
- Public IPv4: 16.170.245.80
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

The `-k` option tells curl to skip certificate verification. That is why this command works:

```bash


