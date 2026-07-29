# Wazuh SIEM Installation

## Project Overview

After preparing the virtual infrastructure and installing the operating systems, the next phase was deploying the Wazuh Security Information and Event Management (SIEM) platform.

Wazuh serves as the central monitoring system within the SOC laboratory. It collects logs from monitored endpoints, analyses security events, detects suspicious activity using predefined and custom rules, and presents alerts through a web-based dashboard.

The assisted installation method was used because it automates the deployment and configuration of all required Wazuh components, ensuring compatibility between services.

---

# Objectives

The objectives of this phase were to:

- Install the Wazuh platform on Ubuntu Server.
- Deploy all required Wazuh components.
- Verify that each service started successfully.
- Access the Wazuh Dashboard.
- Prepare the SIEM for endpoint enrolment.

---

# Software Used

| Software | Version |
|----------|---------|
| Ubuntu Server | 24.04 LTS |
| Wazuh | 4.14 |
| Filebeat | Installed by Wazuh |
| OpenSearch Indexer | Installed by Wazuh |
| OpenSearch Dashboard (Wazuh Dashboard) | Installed by Wazuh |

---

# What is Wazuh?

Wazuh is an open-source Security Information and Event Management (SIEM) and Extended Detection and Response (XDR) platform.

It enables security teams to:

- Collect logs from endpoints.
- Detect suspicious activity.
- Monitor file integrity.
- Detect malware.
- Monitor system configurations.
- Analyse authentication events.
- Generate security alerts.
- Visualise security events through dashboards.

Within this SOC lab, Wazuh acts as the central security monitoring platform.

---

# Installation Method

The assisted installation script was used.

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
chmod +x wazuh-install.sh
sudo ./wazuh-install.sh -a
```

---

# Command Explanation

## Download the Installer

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
```

### What this command does

- `curl` downloads files from a URL.
- `-s` runs in silent mode, hiding download progress.
- `-O` saves the file using its original filename.

The command downloads the official Wazuh installation script.

---

## Make the Script Executable

```bash
chmod +x wazuh-install.sh
```

### What this command does

`chmod` changes file permissions.

The `+x` option adds execute permission, allowing the script to be run as a program.

Without this permission, Linux would refuse to execute the script.

---

## Run the Installer

```bash
sudo ./wazuh-install.sh -a
```

### What this command does

- `sudo` runs the script with administrator privileges.
- `./` tells Linux to execute the script from the current directory.
- `-a` performs an assisted installation.

The installer automatically downloads, installs, and configures all required Wazuh components.

---

# Components Installed

The assisted installation deploys several services that work together to form the SIEM platform.

## Wazuh Manager

The Wazuh Manager is responsible for:

- Receiving logs from agents.
- Analysing security events.
- Applying detection rules.
- Generating alerts.
- Managing enrolled endpoints.

Think of the manager as the "brain" of the SIEM.

---

## Wazuh Indexer

The Indexer stores all collected events.

It indexes incoming logs, making them searchable and enabling fast queries within the dashboard.

Without the Indexer, logs cannot be stored or searched efficiently.

---

## Filebeat

Filebeat transfers log data between the Wazuh Manager and the Indexer.

Its primary responsibility is forwarding processed security events for storage.

---

## Wazuh Dashboard

The Dashboard provides a web interface for interacting with the SIEM.

From the dashboard it is possible to:

- View enrolled agents.
- Investigate alerts.
- Search logs.
- Create dashboards.
- Review security events.
- Monitor endpoint health.

---

# Service Verification

After installation, each service was verified using `systemctl`.

Example:

```bash
sudo systemctl status wazuh-manager
```

The same process was repeated for:

```bash
sudo systemctl status wazuh-indexer
```

```bash
sudo systemctl status filebeat
```

```bash
sudo systemctl status wazuh-dashboard
```

---

## Understanding the Command

```bash
systemctl status <service>
```

### What this command does

`systemctl` is the Linux utility used to manage system services.

The `status` option displays information such as:

- Whether the service is running.
- Service uptime.
- Process ID (PID).
- Recent log messages.
- Startup status.

A successful installation should display:

```
Active: active (running)
```

for each service.

---

# Network Verification

Before accessing the dashboard, the server's IP address was confirmed.

```bash
ifconfig
```

or

```bash
hostname -I
```

### Why this is important

The dashboard is accessed from another machine using the server's IP address.

Example:

```
https://<Ubuntu-Server-IP>
```

In this lab, the Ubuntu server's IP address was used to access the Wazuh web interface over HTTPS.

---

# Accessing the Wazuh Dashboard

The Wazuh Dashboard was accessed from a web browser using:

```
https://<Ubuntu-Server-IP>
```

Because Wazuh uses a self-signed SSL certificate by default, the browser displayed a security warning.

After confirming the connection, the login page loaded successfully.

---

# Verification Checklist

The Wazuh installation was considered successful after confirming:

- Ubuntu Server booted successfully.
- Wazuh Manager running.
- Wazuh Indexer running.
- Filebeat running.
- Wazuh Dashboard running.
- Server IP address verified.
- Dashboard accessible via HTTPS.
- Login page displayed successfully.

---

# Challenges Encountered

## Service Verification

Rather than assuming the installation completed successfully, each service was verified individually using `systemctl status`.

This helped identify whether any component had failed before proceeding to endpoint enrolment.

---

## Network Connectivity

The server's IP address was verified before accessing the dashboard.

This ensured the dashboard could be reached from other virtual machines within the lab environment.

---

## HTTPS Certificate Warning

The browser displayed a certificate warning because Wazuh uses a self-signed certificate during the initial installation.

For this lab environment, the warning was expected and the connection was trusted to continue accessing the dashboard.

---

# Skills Developed

During this phase I gained practical experience with:

- SIEM deployment
- Linux service management
- Wazuh architecture
- HTTPS access
- Service verification
- Log management infrastructure
- Security monitoring platforms

---

# Lessons Learned

Deploying a SIEM involves more than simply running an installation script. Verifying that every component is operational is essential before onboarding endpoints.

Understanding the role of each Wazuh component—Manager, Indexer, Filebeat, and Dashboard—provides a strong foundation for troubleshooting and expanding the platform as additional endpoints and telemetry sources are added.

---

# Technologies Used

- Ubuntu Server
- Wazuh 4.14
- Filebeat
- OpenSearch Indexer
- OpenSearch Dashboard
- Linux Systemd
- HTTPS

---

# Next Phase

With Wazuh successfully installed and operational, the next phase focuses on enrolling Windows Server and Kali Linux as monitored endpoints. This enables the SIEM to begin collecting logs, generating alerts, and providing visibility into endpoint activity across the SOC laboratory.
