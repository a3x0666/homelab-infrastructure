# UFW Firewall Configuration

## Overview

UFW (Uncomplicated Firewall) was deployed to provide host-based firewall protection for the Ubuntu Server.

The firewall acts as the first layer of defense by controlling inbound and outbound network traffic and restricting access to only the services required by the homelab environment.

---

## Objectives

The primary goals of deploying UFW were:

* Restrict unauthorized network access
* Reduce the attack surface of the server
* Allow only required services
* Improve overall security posture
* Gain experience with firewall administration

---

## Architecture Position

```text
                Network Traffic
                        │
                        ▼
                UFW Firewall
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
     SSH (22)      HTTP (80)      SMB (445)
        │               │               │
        ▼               ▼               ▼
     Ubuntu         CasaOS          Samba
```

All incoming traffic is evaluated against firewall rules before reaching the hosted services.

---

## Why UFW?

Linux systems can expose multiple services to the network.

Without a firewall:

```text
Internet / LAN
      │
      ▼
 All Open Services
```

With UFW:

```text
Internet / LAN
      │
      ▼
     UFW
      │
      ▼
Allowed Services Only
```

This follows the principle of least privilege.

---

## Installation

UFW is included by default with Ubuntu Server.

Verify installation:

```bash
sudo ufw version
```

---

## Initial Configuration

### Default Policies

Configure secure defaults:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

This configuration:

* Blocks all incoming traffic by default
* Allows outbound connections
* Requires explicit rules for each service

---

## Allow Required Services

### SSH Access

Allow remote administration:

```bash
sudo ufw allow ssh
```

Equivalent:

```bash
sudo ufw allow 22/tcp
```

---

### CasaOS Web Interface

Allow HTTP traffic:

```bash
sudo ufw allow 80/tcp
```

This permits access to:

```text
http://<server-ip>
```

---

### Samba File Sharing

Allow SMB traffic:

```bash
sudo ufw allow samba
```

This enables access to network shares from Windows systems.

---

## Enable Firewall

After configuring the required rules:

```bash
sudo ufw enable
```

Expected output:

```text
Firewall is active and enabled on system startup
```

---

## Verification

### Check Firewall Status

```bash
sudo ufw status
```

Example:

```text
22/tcp     ALLOW
80/tcp     ALLOW
Samba      ALLOW
```

---

### Detailed Status

```bash
sudo ufw status verbose
```

This displays:

* Default policies
* Active rules
* Logging status

---

### Numbered Rules

```bash
sudo ufw status numbered
```

Example:

```text
[1] 22/tcp ALLOW IN Anywhere
[2] 80/tcp ALLOW IN Anywhere
[3] Samba ALLOW IN Anywhere
```

Numbered rules simplify rule management.

---

## Rule Management

### Remove a Rule

Example:

```bash
sudo ufw delete 2
```

This removes rule number 2.

---

### Add a Rule

```bash
sudo ufw allow 3001/tcp
```

Example use case:

* Uptime Kuma

---

### Block a Port

```bash
sudo ufw deny 80/tcp
```

This immediately blocks HTTP traffic.

---

## Troubleshooting

### CasaOS Became Inaccessible

#### Symptoms

* Web interface unavailable
* Browser unable to connect
* CasaOS service appeared healthy

#### Investigation

Verify service status:

```bash
sudo systemctl status casaos
```

Verify listening ports:

```bash
sudo ss -tulpn | grep :80
```

Result:

```text
CasaOS was running normally.
```

#### Root Cause

Port 80 was not permitted through UFW.

Although CasaOS was operational, the firewall prevented access.

#### Resolution

Allow HTTP traffic:

```bash
sudo ufw allow 80/tcp
```

Verify:

```bash
sudo ufw status
```

Access was restored immediately.

---

### Service Running But Unreachable

When troubleshooting:

1. Verify the service is running.
2. Verify the service is listening on a port.
3. Verify firewall rules.
4. Verify network connectivity.

Useful commands:

```bash
sudo systemctl status <service>
```

```bash
sudo ss -tulpn
```

```bash
sudo ufw status
```

---

## Current Rule Set

Current allowed services:

| Service | Port |
| ------- | ---- |
| SSH     | 22   |
| CasaOS  | 80   |
| Samba   | 445  |

Additional services may be added as the homelab expands.

---

## Security Considerations

Current protections:

* Default deny inbound policy
* Explicit allow rules
* No unnecessary exposed services
* Integration with Tailscale remote access

Additional best practices:

* Review firewall rules regularly
* Remove unused services
* Avoid opening ports unnecessarily
* Prefer VPN access over port forwarding

---

## Outcome

The UFW firewall provides:

* Host-based protection
* Controlled network access
* Reduced attack surface
* Service-level traffic filtering

The deployment establishes a strong security baseline for the homelab environment.

---

## Skills Demonstrated

This deployment involved:

* Firewall Administration
* Network Security
* Linux System Administration
* Service Hardening
* Port Management
* Network Troubleshooting

These skills are fundamental to system administration, infrastructure management, and cybersecurity operations.
