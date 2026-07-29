# Windows Server 2022 Installation

## Project Overview

Windows Server 2022 was deployed as a monitored endpoint within the SOC laboratory. The server simulates a Windows-based enterprise system that generates security logs, system events, and authentication activity for monitoring by the Wazuh SIEM platform.


# Objectives

The objectives of this phase were to:

- Install Windows Server 2022 in Oracle VirtualBox.
- Configure the initial operating system settings.
- Enable remote administration features.
- Install VirtualBox Guest Additions.
- Enable clipboard sharing between the host and guest.
- Prepare the endpoint for future Wazuh agent installation.

---

# Software Used

| Software | Version |
|----------|---------|
| Windows Server 2022 Evaluation | Desktop Experience |
| Oracle VirtualBox | Latest Stable Release |
| VirtualBox Guest Additions | Matching VirtualBox Version |

---

# Why Windows Server 2022?

Windows Server was selected because it closely represents enterprise environments monitored by Security Operations Centers (SOCs).

Using Windows Server allows the generation of realistic security events, including:

- Authentication events
- User account management
- PowerShell activity
- Windows Defender alerts
- Event Viewer logs
- Sysmon telemetry
- Active Directory events 

These logs provide valuable data for detection engineering and incident response.

---

# Installation Process

## 1. Download Windows Server 2022

The Windows Server 2022 Evaluation ISO was downloaded from Microsoft's Evaluation Center.

The evaluation edition provides a fully functional environment suitable for learning and testing.

---

## 2. Create the Virtual Machine

A new virtual machine was created in Oracle VirtualBox using the following specifications.

| Resource | Allocation |
|----------|------------|
| CPUs | 2 vCPUs |
| Memory | 4 GB RAM |
| Storage | 50 GB Virtual Disk |
| Network | Internal Network |

---

## 3. Install Windows Server

The virtual machine was booted using the downloaded ISO.

During installation, Windows Setup prompted for the operating system edition.

### Initial Mistake

Initially, the following option was selected:

```
Windows Server 2022 Standard Evaluation
```

This version installs **Server Core**, which provides only a command-line interface without a graphical desktop.

While Server Core is commonly used in production environments due to its smaller attack surface and reduced resource usage, it was not suitable for this learning environment where graphical tools would simplify configuration and troubleshooting.

### Resolution

The installation was repeated using:

```
Windows Server 2022 Standard Evaluation (Desktop Experience)
```

This version includes the full graphical user interface (GUI), making it more suitable for learning Windows administration and interacting with security tools.

---

## 4. Initial Configuration

After installation, the following configuration tasks were completed:

- Administrator password configured.
- Time zone verified.
- Computer name reviewed.
- Network connectivity confirmed.
- Successful login to the Windows desktop.

---

# VirtualBox Guest Additions

Initially, copying and pasting between the host operating system and the Windows virtual machine was not possible.

To improve usability, Oracle VirtualBox Guest Additions were installed.

Guest Additions provide enhanced integration between the host and guest operating systems, including:

- Shared clipboard
- Drag and drop
- Improved display support
- Better mouse integration
- Shared folders

After installation, the virtual machine was rebooted.

---

# Enabling Clipboard Sharing

Once Guest Additions were installed, VirtualBox settings were updated.

The following options were enabled:

```
Devices
    → Shared Clipboard
        → Bidirectional
```

and

```
Devices
    → Drag and Drop
        → Bidirectional
```

This allowed commands, configuration files, and documentation to be copied seamlessly between the host computer and the Windows virtual machine.

---

# System Verification

Before proceeding to endpoint enrolment, the operating system was verified.

The following checks were performed:

- Successful boot.
- Administrator login.
- Network connectivity.
- Assigned IP address.
- Available disk space.
- Available memory.
- Guest Additions installed.
- Clipboard sharing functioning correctly.

Useful PowerShell commands included:

```powershell
hostname
```

Displays the computer name.

---

```powershell
ipconfig
```

Displays network interface configuration and assigned IP addresses.

---

```powershell
systeminfo
```

Displays detailed operating system and hardware information.

---

```powershell
Get-Service
```

Lists installed Windows services and their current status.

---

# Challenges Encountered

## Incorrect Installation Edition

The first installation used the Server Core edition instead of Desktop Experience.

### Resolution

The operating system was reinstalled using the Desktop Experience edition, providing access to the graphical interface required for the remainder of the lab.

---

## Clipboard Sharing

Initially, copy-and-paste functionality between the host and guest operating systems was unavailable.

### Resolution

VirtualBox Guest Additions were installed, followed by enabling bidirectional clipboard and drag-and-drop functionality in the VirtualBox settings.

---

## Network Verification

The endpoint's network configuration was verified to ensure communication with the Ubuntu Server hosting Wazuh.

This step is essential because endpoint enrolment requires reliable communication between the agent and the SIEM server.

---

# Verification Checklist

The Windows Server installation was considered complete after confirming:

- Windows Server booted successfully.
- Desktop Experience installed.
- Administrator account configured.
- Network connectivity established.
- VirtualBox Guest Additions installed.
- Shared clipboard functioning correctly.
- Endpoint ready for Wazuh agent installation.

---

# Skills Developed

During this phase I gained practical experience with:

- Windows Server installation
- VirtualBox configuration
- Windows administration
- PowerShell
- Guest Additions installation
- Enterprise endpoint preparation
- Troubleshooting virtual environments

---

# Lessons Learned

Selecting the correct Windows Server edition during installation is an important decision.

While Server Core offers improved security and lower resource consumption, the Desktop Experience edition is more appropriate for learning environments where graphical administration tools simplify configuration and troubleshooting.

Installing VirtualBox Guest Additions significantly improves productivity by enabling seamless interaction between the host and guest operating systems.

---

# Technologies Used

- Windows Server 2022 Evaluation
- Oracle VirtualBox
- VirtualBox Guest Additions
- Windows PowerShell

---

# Next Phase

With Windows Server successfully installed and configured, the next phase focuses on enrolling the endpoint into the Wazuh SIEM platform, allowing Windows event logs to be collected, analysed, and monitored for security events.
