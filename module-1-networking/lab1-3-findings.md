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
 ``` 
wg genkey | sudo tee /etc/wireguard/server_private.key | wg pubkey | sudo tee /etc/wireguard/server_public.key
sudo cat /etc/wireguard/server_public.key
```
VM1 public key: HRqcenbsqrWSl4MHznf1NhiyValKi4nP5TCUjOxNbiM=
- VM2 Client key generation command:
 ```
wg genkey | sudo tee /etc/wireguard/client_private.key | wg pubkey | sudo tee /etc/wireguard/client_public.key
sudo cat /etc/wireguard/client_public.key
```
VM2 public key: TZX9fMlmL0nvERZ8attnuvGKw5RXkXK2yu66b6cCdEw=

Explanation
WireGuard uses public and private key pairs for authentication.
The private key stays secret on each VM, while the public key is shared with the peer VM.
ss

## Experiment 3: WireGuard Interface Before Handshake
Command used on VM1: sudo wg show
Output observed
Before VM2 connected, VM1 showed the WireGuard interface and peer details, but no active handshake yet.

Screenshot
##Experiment 4: WireGuard Active Handshake
Command used on VM1 and VM2
```
sudo wg show
```
-VM1 output summary

VM1 showed VM2 as a peer with: 
```text
endpoint: 51.20.65.72:57182
allowed ips: 10.0.0.2/32
latest handshake: 37 seconds ago
transfer: 892 B received, 276 B sent
```
-VM2 output summary

VM2 showed VM1 as a peer with:
```text
endpoint: 16.170.245.80:51820
allowed ips: 10.0.0.1/32
latest handshake: 21 seconds ago
transfer: 92 B received, 180 B sent
persistent keepalive: every 25 seconds
```
Explanation

The latest handshake confirms that both VMs successfully authenticated each other and the WireGuard tunnel became active.

Screenshots

## Experiment 5: Tunnel Ping Test
Command used on VM2
```
ping -c 4 10.0.0.1
```
-Output observed
```text
4 packets transmitted, 4 received, 0% packet loss
rtt min/avg/max/mdev = 0.363/0.373/0.383/0.007 ms
```
Explanation

The IP address 10.0.0.1 is the WireGuard tunnel IP of VM1.
This ping was sent from VM2 to VM1 through the encrypted WireGuard tunnel.

The successful ping confirms that VM2 can reach VM1 using the private VPN tunnel network.

Screenshot

## Experiment 6: Encrypted UDP Traffic Capture
-Command used on VM1
```
sudo tcpdump -i any udp port 51820
```
-Traffic generated from VM2
```
ping -c 4 10.0.0.1
```
-Output observed
Tcpdump showed UDP traffic on port 51820 between VM1 and VM2.
-Summary observed:
```text
35 packets captured
36 packets received by filter
0 packets dropped by kernel
```
Explanation

WireGuard uses UDP port 51820 for tunnel communication.
The tcpdump output showed UDP packets, but the actual ping data was not readable in plain text.

This proves that the traffic between VM1 and VM2 was encrypted inside the WireGuard tunnel.

Screenshots

##WireGuard to ZTNA Component Mapping

| WireGuard Lab Component | ZTNA Component                               |
| ----------------------- | -------------------------------------------- |
| VM2 Client              | User device / Agent                          |
| VM1 Server              | Gateway                                      |
| WireGuard tunnel        | Encrypted ZTNA tunnel                        |
| Public/private keys     | Certificate or identity-based authentication |
| AllowedIPs              | Access policy / permitted routes             |
| Handshake               | Secure session establishment                 |
| UDP 51820 traffic       | Encrypted tunnel traffic                     |




