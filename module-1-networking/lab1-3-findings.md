# Lab 1.3 Findings

## Lab Title

WireGuard VPN — Build Your Own Encrypted Tunnel

---

## My VM Details

### VM1 — Server / Gateway

- Provider: AWS
- Region: eu-north-1
- OS: Ubuntu
- Public IPv4: 16.170.245.80
- WireGuard tunnel IP: 10.0.0.1
- Role: VPN Server / Gateway

### VM2 — Client / Agent

- Provider: AWS
- Region: eu-north-1
- OS: Ubuntu
- Public IPv4: 51.20.65.72
- WireGuard tunnel IP: 10.0.0.2
- Role: VPN Client / Agent

---

## Experiment 1: WireGuard Installation

### Command used on both VMs

```bash
sudo apt update && sudo apt install -y wireguard
wg --version
```

Output observed

WireGuard was successfully installed on both VM1 and VM2.
Version observed:
ss
## Experiment 2: Key Generation on Both VMs
- VM1 Server key generation command: 
wg genkey | sudo tee /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key
sudo cat /etc/wireguard/server_public.key
VM1 public key: HRqcenbsqrWSl4MHznf1NhiyValKi4nP5TCUjOxNbiM=
- VM2 Client key generation command: 
wg genkey | sudo tee /etc/wireguard/client_private.key | wg pubkey | sudo tee /etc/wireguard/client_public.key
sudo cat /etc/wireguard/client_public.key
VM2 public key: TZX9fMlmL0nvERZ8attnuvGKw5RXkXK2yu66b6cCdEw=

Explanation
WireGuard uses public and private key pairs for authentication.
The private key stays secret on each VM, while the public key is shared with the peer VM.
ss

##Experiment 3: WireGuard Interface Before Handshake
Command used on VM1: sudo wg show
Output observed
Before VM2 connected, VM1 showed the WireGuard interface and peer details, but no active handshake yet.

Screenshot


