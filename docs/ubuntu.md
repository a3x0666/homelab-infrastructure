# Ubuntu Server Installation

## Overview

This document outlines the installation and initial configuration of Ubuntu Server 24.04 LTS on an HP laptop repurposed as a home server.

### Hardware Specifications

| Component        | Specification           |
| ---------------- | ----------------------- |
| Device           | HP Laptop               |
| Storage          | 1 TB HDD                |
| Memory           | 4 GB RAM                |
| Operating System | Ubuntu Server 24.04 LTS |

> **Important:** During the Ubuntu Server installation process, ensure that the **OpenSSH Server** option is selected. This allows remote administration of the server after installation.

---

## Laptop-Specific Configuration

Since this server is hosted on a laptop, the default power management settings must be adjusted to prevent the system from suspending when the lid is closed.

### Step 1: Disable Suspend on Lid Close

Edit the systemd logind configuration file:

```bash
sudo nano /etc/systemd/logind.conf
```

Locate and update the following parameters:

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
LidSwitchIgnoreInhibited=no
```

Save the file and reboot the server:

```bash
sudo reboot
```

This ensures the server remains operational even when the laptop lid is closed.

---

## Step 2: Initial Server Setup

After the server reboots, connect to it from another machine using SSH.

### Connect via SSH

```bash
ssh <username>@<server_ip_address>
```

Replace:

* `<username>` with the user account created during installation.
* `<server_ip_address>` with the server's local IP address.

### Update System Packages

Once connected:

```bash
sudo apt update && sudo apt upgrade -y
```

After all packages have been updated, reboot the server:

```bash
sudo reboot
```

Reconnect using SSH once the server is back online.

---

## Step 3: Install CasaOS

CasaOS provides a simple web-based interface for managing applications, storage, and services on the server.

### Installation

Run the following command:

```bash
curl -fsSL https://get.casaos.io | sudo bash
```

The installation process typically completes within 5–10 minutes.

### Accessing CasaOS

After installation, open a web browser and navigate to:

```text
http://<server_ip_address>
```

Example:

```text
http://192.168.1.10
```

### Initial Configuration

1. Open the CasaOS web interface.
2. Create an administrator account.
3. Configure login credentials.
4. Complete the setup wizard.

Once completed, the server is ready for application deployment and further homelab configuration.

---

## Outcome

The server now provides:

* Remote administration via SSH
* Web-based management through CasaOS
* Persistent operation with the laptop lid closed
* A foundation for hosting services such as:

  * Samba (SMB)
  * Uptime Kuma
  * Tailscale
  * Pi-hole (planned)
  * Additional Docker containers

This serves as the base infrastructure for the homelab environment documented in this repository.
