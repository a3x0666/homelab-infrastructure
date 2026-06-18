# Uptime Kuma Monitoring and Alerting

## Overview

Uptime Kuma was deployed as the primary monitoring solution for the homelab environment.

The platform continuously monitors critical services and sends notifications when a service becomes unavailable.

This provides visibility into the health of the infrastructure and reduces the time required to identify service outages.

---

## Objectives

The primary goals of deploying Uptime Kuma were:

* Monitor critical services
* Receive outage notifications
* Improve infrastructure visibility
* Learn monitoring fundamentals
* Implement basic observability practices

---

## Architecture Position

```text
                          Discord
                              ▲
                              │
                    Alert Notifications
                              ▲
                              │
                        Uptime Kuma
                              │
            ┌─────────────────┼─────────────────┐
            │                 │                 │
            ▼                 ▼                 ▼
         CasaOS           SMB Share       Internet
        HTTP(80)          TCP(445)         Ping
```

Uptime Kuma continuously checks service availability and generates alerts when a monitored service becomes unavailable.

---

## Why Monitoring Matters

Without monitoring:

```text
Service Failure
      │
      ▼
User discovers issue later
```

With monitoring:

```text
Service Failure
      │
      ▼
Uptime Kuma Detection
      │
      ▼
Discord Notification
      │
      ▼
Immediate Awareness
```

This significantly reduces response time during outages.

---

## Deployment Method

Uptime Kuma was deployed using Docker through CasaOS.

Container image:

```text
louislam/uptime-kuma
```

---

## Verify Container Status

Check running containers:

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE
xxxxxxxxxxxx   louislam/uptime-kuma
```

---

## Accessing Uptime Kuma

Default web interface:

```text
http://<server-ip>:3001
```

Example:

```text
http://192.168.1.10:3001
```

---

## Initial Configuration

### Administrator Account

Upon first launch:

1. Create an administrator account.
2. Configure a secure password.
3. Complete initial setup.

---

## Creating Monitors

### HTTP Monitor

Used for web services.

Example:

```text
Type: HTTP(s)

Name:
CasaOS

URL:
http://192.168.1.10
```

Purpose:

* Monitor CasaOS availability
* Detect web service failures

---

### TCP Port Monitor

Used for network services.

Example:

```text
Type: TCP Port

Host:
192.168.1.10

Port:
445
```

Purpose:

* Verify SMB availability
* Detect file sharing outages

---

### Ping Monitor

Used for network connectivity checks.

Example:

```text
Type: Ping

Host:
1.1.1.1
```

Purpose:

* Detect internet outages
* Verify network connectivity

---

## Notification Configuration

### Discord Integration

Discord was selected as the primary alerting platform.

---

### Create Discord Webhook

1. Open Discord Server Settings.
2. Navigate to Integrations.
3. Select Webhooks.
4. Create a new webhook.
5. Assign a notification channel.
6. Copy the webhook URL.

---

### Configure Discord Notification

In Uptime Kuma:

```text
Settings
    ↓
Notifications
    ↓
Setup Notification
    ↓
Discord
```

Provide:

* Notification Name
* Discord Webhook URL

---

### Test Notifications

Use:

```text
Test
```

A successful test confirms connectivity between Uptime Kuma and Discord.

---

## Attaching Notifications to Monitors

Creating a notification does not automatically assign it to monitors.

For each monitor:

```text
Edit Monitor
    ↓
Notifications
    ↓
Select Discord Notification
    ↓
Save
```

Once attached, alerts will be sent whenever the monitor changes state.

---

## Monitoring Targets

Current monitored services:

### CasaOS

```text
HTTP
http://<server-ip>
```

Purpose:

* Verify web interface availability

---

### SMB File Sharing

```text
TCP Port
Port 445
```

Purpose:

* Verify Samba accessibility

---

### Internet Connectivity

```text
Ping
1.1.1.1
```

Purpose:

* Detect ISP or network outages

---

### Uptime Kuma

```text
HTTP
http://<server-ip>:3001
```

Purpose:

* Verify monitoring platform availability

---

## Verification

### Confirm Container Health

```bash
docker ps
```

Expected:

```text
STATUS
Up (healthy)
```

---

### Verify Port Availability

```bash
sudo ss -tulpn | grep 3001
```

Expected:

```text
0.0.0.0:3001
```

---

## Alert Testing

### Method 1: Invalid Endpoint

Create a test monitor:

```text
Type:
HTTP

URL:
http://<server-ip>:9999
```

Expected result:

```text
DOWN
```

Discord notification should be generated automatically.

---

### Method 2: Service Interruption

Temporarily stop a monitored service:

```bash
sudo systemctl stop <service>
```

Expected:

* Monitor status changes to DOWN
* Discord notification generated

Restart:

```bash
sudo systemctl start <service>
```

Expected:

* Monitor status changes to UP
* Recovery notification generated

---

## Troubleshooting

### Notifications Not Received

Verify:

1. Discord webhook is valid.
2. Notification test succeeds.
3. Notification is attached to the monitor.

Common mistake:

```text
Notification Created
    ✓

Notification Assigned
    ✗
```

---

### Monitor Remains Online

Verify monitor type.

Example:

```text
Ping Monitor
```

only verifies server reachability.

It does not verify application functionality.

For application monitoring, use:

```text
HTTP Monitor
```

instead.

---

### False Positives

Review:

* Monitor interval
* Timeout settings
* Retry count

Adjust thresholds as necessary.

---

## Security Considerations

Current protections:

* Internal access only
* Discord webhook secured
* No public exposure required
* Monitoring integrated with Tailscale-protected infrastructure

Future enhancements:

* Multi-channel alerting
* Status pages
* Performance monitoring
* Centralized logging

---

## Outcome

Uptime Kuma successfully provides:

* Service monitoring
* Outage detection
* Discord alerting
* Infrastructure visibility
* Basic observability capabilities

The deployment improves reliability by enabling rapid detection of service interruptions.

---

## Skills Demonstrated

This deployment involved:

* Monitoring and Observability
* Docker Administration
* Alerting Systems
* Incident Detection
* Service Health Monitoring
* Infrastructure Operations

These skills are commonly used in system administration, DevOps, Site Reliability Engineering (SRE), and cybersecurity environments.
