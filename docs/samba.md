# Samba (SMB) File Sharing Configuration

## Overview

Samba was configured on the Ubuntu Server to provide centralized network file sharing across devices on the local network.

This allows Windows systems to access files stored on the home server using the SMB (Server Message Block) protocol.

The objective was to transform the server into a lightweight NAS (Network Attached Storage) solution while maintaining ease of access from Windows Explorer.

---

## Objectives

The primary goals of deploying Samba were:

* Create a centralized file repository
* Enable file sharing between Windows and Linux systems
* Utilize the server's 1 TB storage as a network drive
* Learn SMB networking fundamentals
* Build the foundation of a home NAS solution

---

## Architecture Position

```text
Windows PC
     │
     │ SMB (445)
     ▼
Ubuntu Server
     │
     ▼
Samba Service
     │
     ▼
Shared Directory
```

The Samba service acts as the bridge between Windows clients and Linux storage.

---

## Installation

### Update Package Lists

```bash
sudo apt update
```

### Install Samba

```bash
sudo apt install samba -y
```

Verify installation:

```bash
smbd --version
```

---

## Service Verification

Verify that Samba is running:

```bash
sudo systemctl status smbd
```

Expected output:

```text
Active: active (running)
```

Enable automatic startup:

```bash
sudo systemctl enable smbd
```

---

## Share Directory Configuration

### Create Shared Directory

Example:

```bash
mkdir -p ~/shared
```

Adjust ownership:

```bash
sudo chown -R $USER:$USER ~/shared
```

Adjust permissions:

```bash
chmod 775 ~/shared
```

---

## Configure Samba Share

Edit the Samba configuration file:

```bash
sudo nano /etc/samba/smb.conf
```

Append the following section:

```ini
[Shared]
path = /home/<username>/shared
browseable = yes
writable = yes
guest ok = no
read only = no
create mask = 0775
directory mask = 0775
```

Replace:

```text
<username>
```

with the server account username.

---

## Create Samba User

Add the Linux user to Samba:

```bash
sudo smbpasswd -a <username>
```

Create a password when prompted.

Enable the account:

```bash
sudo smbpasswd -e <username>
```

---

## Restart Samba

Apply changes:

```bash
sudo systemctl restart smbd
```

Verify service health:

```bash
sudo systemctl status smbd
```

---

## Firewall Configuration

Allow SMB traffic through UFW:

```bash
sudo ufw allow samba
```

Verify:

```bash
sudo ufw status
```

Expected output:

```text
Samba ALLOW Anywhere
```

---

## Verification

### Check Listening Ports

```bash
sudo ss -tulpn | grep smbd
```

Expected ports:

```text
445/tcp
139/tcp
```

---

### Access From Windows

Open File Explorer.

Navigate to:

```text
\\<server-ip-address>
```

Example:

```text
\\192.168.1.10
```

The shared folder should appear.

Authenticate using:

```text
Username: <server_username>
Password: <samba_password>
```

---

## Mapping a Network Drive

Optional: map the share as a permanent network drive.

Windows Explorer:

```text
This PC
    ↓
Map Network Drive
```

Folder:

```text
\\192.168.1.10\Shared
```

Enable:

```text
Reconnect at sign-in
```

This allows the share to function similarly to a local drive.

---

## Troubleshooting

### Share Not Visible

Verify Samba service:

```bash
sudo systemctl status smbd
```

Verify configuration:

```bash
testparm
```

---

### Authentication Failure

Verify Samba account exists:

```bash
sudo pdbedit -L
```

Reset password:

```bash
sudo smbpasswd <username>
```

---

### Cannot Connect From Windows

Verify firewall:

```bash
sudo ufw status
```

Confirm Samba rule exists:

```bash
sudo ufw allow samba
```

Verify listening ports:

```bash
sudo ss -tulpn | grep 445
```

---

### Network Discovery Issues

Verify the server IP:

```bash
hostname -I
```

Attempt access directly:

```text
\\<server-ip-address>
```

instead of relying on automatic discovery.

---

## Security Considerations

Current security measures:

* Authentication required
* Firewall rules configured through UFW
* Access limited to local network
* No SMB exposure to the public internet

Additional precautions:

* SMB traffic is not port-forwarded
* Remote access is performed through Tailscale
* User authentication required for access

---

## Outcome

The Ubuntu Server now functions as a lightweight NAS solution.

Capabilities include:

* Centralized file storage
* Windows file sharing
* Local network accessibility
* Secure authenticated access
* Integration with the homelab infrastructure

The deployment successfully transformed the server's 1 TB storage into a network-accessible file repository.

---

## Skills Demonstrated

This deployment involved:

* Linux Administration
* SMB Protocol Configuration
* Network File Sharing
* User Authentication
* Permission Management
* Firewall Configuration
* Network Troubleshooting

These skills are commonly used in enterprise environments where centralized file storage and network-accessible resources are required.
