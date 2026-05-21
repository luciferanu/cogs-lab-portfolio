# Capstone Lab — WireGuard + Nginx Proxy + Monitoring

## Objective

The objective of this capstone lab was to connect two EC2 VMs in a zero-trust style lab architecture, use one VM as an application/backend server, use the second VM as a gateway/reverse proxy, and monitor the proxy path using Uptime Kuma.

---

## Architecture

```text
Laptop / Browser
      |
      | HTTP request
      v
VM2 — Gateway / Nginx Reverse Proxy
Public IP: 13.60.52.60
Private IP: 172.31.41.52
      |
      | Private AWS network
      v
VM1 — App Server / Keycloak / Backend
Public IP: 13.60.163.172
Private IP: 172.31.38.99
WireGuard IP: 10.0.0.1
```
### VM Roles
| VM  | Public IP     | Private IP   | Role                          |
| --- | ------------- | ------------ | ----------------------------- |
| VM1 | 13.60.163.172 | 172.31.38.99 | App server / backend target   |
| VM2 | 13.60.52.60   | 172.31.41.52 | Nginx gateway / reverse proxy |

## Step 1 — VM1 Backend Service

VM1 was used as the backend application server. Nginx was already running on VM1 and served the lab application page.

![Nginx proxy test success](../screenshots/capstone-nginx-proxy-test-success.png)

VM1 backend response:
```text
<h1>COGS Lab - TLS Demo</h1><p>Hello from InstaSafe Support Lab!</p>
```
## Step 2 — VM2 Reverse Proxy Configuration

VM2 was configured with Nginx as a reverse proxy.

The proxy configuration forwarded requests from:

-> http://13.60.52.60/app/

to VM1 over the private AWS network:

-> http://172.31.38.99/

Nginx proxy configuration used:
```text
server {
    listen 80;
    server_name _;
    location /app/ {
        proxy_pass http://172.31.38.99/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    location /health {
        return 200 "capstone gateway healthy\n";
        add_header Content-Type text/plain;
    }
}
```
Nginx configuration was tested successfully and reloaded.

## Step 3 — Proxy Path Validation

The proxy path was tested from VM2 using curl.

### Command used:
```text
curl http://localhost/app/
```
![Proxy terminal proof](../screenshots/capstone-proxy-terminal-proof.png)

### Observed response:

<h1>COGS Lab - TLS Demo</h1><p>Hello from InstaSafe Support Lab!</p>

This confirmed that VM2 was successfully proxying traffic to VM1.

The proxy was also tested from the browser using:

-> http://13.60.52.60/app/

![Proxy app working](../screenshots/capstone-proxy-app-working.png)

## Step 4 — Monitoring with Uptime Kuma

A Uptime Kuma monitor was created to monitor the VM2 proxy URL:

-> http://13.60.52.60/app/

The monitor showed UP/green, confirming that the gateway proxy path was reachable.

![Uptime Kuma proxy green](../screenshots/capstone-uptime-kuma-proxy-green.png)

### Result

The capstone lab successfully connected two EC2 VMs in a gateway/backend architecture.

### Final working flow:

Browser → VM2 Nginx Proxy → VM1 Backend App

VM2 accepted the public HTTP request and forwarded it to VM1 using the private AWS network.

### What I Learned

This capstone helped combine multiple concepts from earlier labs:

EC2 VM networking
Nginx reverse proxy
Private IP communication between VMs
Gateway/backend architecture
Uptime Kuma monitoring
Troubleshooting AWS security group access
Validating service paths using curl and browser testing

The key learning was that the proxy can work locally on the VM but still fail from the browser if AWS Security Group inbound rules do not allow the required port.


