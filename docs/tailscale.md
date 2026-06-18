# Tailscale Remote Access Configuration

## Overview

Tailscale was deployed to provide secure remote access to the homelab infrastructure without exposing services directly to the internet.

By utilizing WireGuard-based encrypted tunnels, Tailscale enables secure connectivity between devices regardless of their physical location or network environment.

This eliminates the need for port forwarding while maintaining secure access to services hosted on the Ubuntu Server.

---

## Objectives

The primary goals of deploying Tailscale were:

* Secure remote access to the server
* Avoid exposing services to the public internet
* Eliminate the need for router port forwarding
* Enable administration from external networks
* Improve the security posture of the homelab

---

## Architecture Position

```text
Remote Device
(Laptop / Phone)
        │
        ▼
    Tailscale
        │
        ▼
Encrypted WireGuard Tunnel
        │
        ▼
Ubuntu Server
        │
        ├── SSH
        ├── CasaOS
        ├── Samba
        └── Docker Services
```

All communication occurs through encrypted tunnels established by Tailscale.

---

## Why Tailscale?

Traditional remote access often requires:

```text
Router
 ├── Port Forward 22 (SSH)
 ├── Port Forward 80 (HTTP)
 └── Port Forward 443 (HTTPS)
```

This increases the server's exposure to the public internet.

Tailscale provides:

* End-to-end encryption
* Device authentication
* Access control
* NAT traversal
* No port forwarding requirements

---

## Installation

### Install Tailscale

Run the official installation script:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

Verify installation:

```bash
tailscale version
```

---

## Authentication

Start Tailscale:

```bash
sudo tailscale up
```

A login URL will be displayed.

Example:

```text
https://login.tailscale.com/...
```

Open the URL in a browser and authenticate using the desired identity provider.

Supported providers include:

* Google
* GitHub
* Microsoft
* Apple

---

## Verification

Check connection status:

```bash
tailscale status
```

Example output:

```text
homeserver    100.x.x.x
phone         100.x.x.x
laptop        100.x.x.x
```

This confirms that devices have successfully joined the Tailscale network.

---

## Determining the Tailscale IP

Display the assigned Tailscale IP:

```bash
tailscale ip -4
```

Example:

```text
100.110.120.130
```

This address can be used from any authenticated Tailscale device.

---

## Remote SSH Access

### Local Network Access

```bash
ssh username@192.168.1.10
```

### Remote Access Through Tailscale

```bash
ssh username@100.x.x.x
```

Example:

```bash
ssh username@100.110.120.130
```

This enables administration from:

* Mobile data
* Public Wi-Fi
* External networks

without exposing SSH to the internet.

---

## Mobile Administration

Tailscale was installed on Android devices to allow remote server administration.

Example workflow:

```text
Android Phone
        │
        ▼
Termux
        │
        ▼
SSH
        │
        ▼
Ubuntu Server
```

This enables:

* Server management
* Service monitoring
* Troubleshooting
* Log inspection

from a mobile device.

---

## Services Accessible Through Tailscale

Current services:

### SSH

```bash
ssh username@100.x.x.x
```

### CasaOS

```text
http://100.x.x.x
```

### Uptime Kuma

```text
http://100.x.x.x:3001
```

### Samba

```text
\\100.x.x.x
```

Access is restricted to authenticated devices within the Tailscale network.

---

## Security Benefits

### No Port Forwarding

Router configuration remains unchanged.

No public exposure of:

* Port 22 (SSH)
* Port 80 (HTTP)
* Port 445 (SMB)
* Port 3001 (Uptime Kuma)

---

### Device Authentication

Only approved devices may access the network.

Unauthorized devices cannot communicate with the server.

---

### End-to-End Encryption

All traffic is encrypted using WireGuard.

This protects:

* Credentials
* Administrative traffic
* File transfers
* Monitoring data

---

## Troubleshooting

### Cannot Reach Server Remotely

Verify Tailscale status:

```bash
tailscale status
```

---

### Device Missing From Network

Re-authenticate:

```bash
sudo tailscale up
```

---

### Unable to SSH

Verify SSH service:

```bash
sudo systemctl status ssh
```

Verify firewall rules:

```bash
sudo ufw status
```

Verify Tailscale connectivity:

```bash
tailscale ping <device-name>
```

---

### Mobile Data Access Issues

Ensure:

* Tailscale is connected on the phone
* Correct Tailscale IP is being used
* SSH service is running

Common mistake:

```bash
ssh username@192.168.1.10
```

This only works on the local network.

Correct method:

```bash
ssh username@100.x.x.x
```

---

## Outcome

Tailscale successfully provides:

* Secure remote administration
* Encrypted connectivity
* Device-based access control
* Mobile server management
* Zero public service exposure

The deployment significantly improves the security and accessibility of the homelab environment.

---

## Skills Demonstrated

This deployment involved:

* VPN Technologies
* WireGuard Fundamentals
* Remote Administration
* Secure Network Design
* Identity-Based Access Control
* Linux Networking
* Security Hardening

These skills are highly relevant to modern infrastructure, cloud, and cybersecurity environments.
