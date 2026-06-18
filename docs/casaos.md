# CasaOS Installation and Configuration

## Overview

CasaOS was deployed on top of Ubuntu Server 24.04 LTS to provide a centralized web-based management platform for the homelab environment.

The platform simplifies storage management, application deployment, Docker container administration, and file management through an intuitive graphical interface.

## Objectives

The primary goals of deploying CasaOS were:

* Simplify server administration
* Manage Docker applications through a web interface
* Monitor system resources
* Manage storage and file shares
* Provide a foundation for self-hosted services

---

## Architecture Position

```text
Ubuntu Server
    │
    ▼
CasaOS
    │
    ▼
Docker Containers
    ├── Uptime Kuma
    ├── Future: Pi-hole
    ├── Future: Netdata
    └── Additional Services
```

CasaOS acts as the management layer between the operating system and containerized applications.

---

## Installation

### Install CasaOS

Run the official installation script:

```bash
curl -fsSL https://get.casaos.io | sudo bash
```

The installation process typically completes within 5–10 minutes.

During installation, CasaOS automatically:

* Installs required dependencies
* Configures Docker
* Deploys core CasaOS services
* Enables automatic startup

---

## Accessing CasaOS

Once installation is complete, open a browser and navigate to:

```text
http://<server-ip-address>
```

Example:

```text
http://192.168.1.10
```

The CasaOS setup page should appear.

---

## Initial Configuration

### Administrator Account

During first launch:

1. Create an administrator account.
2. Configure a secure password.
3. Complete the onboarding process.

### Verification

Verify that the dashboard displays:

* Available storage
* Memory utilization
* CPU information
* System status

For this deployment:

| Resource         | Value                   |
| ---------------- | ----------------------- |
| Storage          | 1 TB                    |
| Memory           | 4 GB RAM                |
| Operating System | Ubuntu Server 24.04 LTS |

---

## Docker Integration

CasaOS uses Docker as its application runtime.

### Verify Docker Installation

```bash
docker ps
```

Expected result:

```text
CONTAINER ID   IMAGE   STATUS
```

### Verify Docker Service

```bash
sudo systemctl status docker
```

Expected state:

```text
active (running)
```

Docker serves as the foundation for all applications installed through CasaOS.

---

## Core Features Utilized

### Application Management

Applications can be installed directly through the CasaOS App Store.

Current deployment:

* Uptime Kuma

Planned deployments:

* Pi-hole
* Netdata
* Wazuh

---

### File Management

CasaOS provides a web-based file manager for:

* Uploading files
* Organizing directories
* Managing storage
* Monitoring disk utilization

---

### Storage Management

The platform automatically detected and mounted the available 1 TB storage device.

Storage can be managed directly through the CasaOS dashboard without requiring command-line interaction.

---

## Service Verification

### Verify CasaOS Service Status

```bash
sudo systemctl status casaos
```

Expected output:

```text
Active: active (running)
```

---

### Verify Web Interface Port

```bash
sudo ss -tulpn | grep :80
```

Expected output:

```text
*:80
```

This confirms that the CasaOS gateway is listening for incoming HTTP connections.

---

## Troubleshooting

### CasaOS Web Interface Not Accessible

#### Symptoms

* Browser cannot connect
* Dashboard unavailable
* HTTP requests fail

#### Investigation

Verify service status:

```bash
sudo systemctl status casaos
```

Verify listening ports:

```bash
sudo ss -tulpn | grep :80
```

---

### Firewall Blocking Access

#### Symptoms

CasaOS service appears healthy but the web interface is inaccessible.

#### Cause

Port 80 was blocked by the UFW firewall.

#### Resolution

Allow HTTP traffic:

```bash
sudo ufw allow 80/tcp
```

Verify rules:

```bash
sudo ufw status
```

#### Lesson Learned

A running service does not guarantee accessibility.

When troubleshooting connectivity issues, verify:

1. Service status
2. Listening ports
3. Firewall configuration
4. Network connectivity

---

## Security Considerations

Current protections include:

* UFW Firewall
* Tailscale Remote Access
* No router port forwarding
* Local network access controls

Future improvements:

* Reverse Proxy
* HTTPS Certificates
* Stronger authentication controls
* Additional monitoring

---

## Outcome

CasaOS successfully provides:

* Centralized server management
* Docker application deployment
* Storage management
* File management
* Simplified administration

The platform serves as the primary management interface for the homelab environment and forms the foundation for all future self-hosted services.

---

## Skills Demonstrated

This deployment involved:

* Linux Administration
* Docker Fundamentals
* Service Management
* Firewall Troubleshooting
* Network Diagnostics
* Self-Hosted Infrastructure

These skills form the foundation of modern system administration and homelab environments.
