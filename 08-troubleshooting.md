# Troubleshooting Journal

## Overview

Throughout the deployment process, several technical challenges were encountered that required troubleshooting, research, and systematic problem solving.

This journal documents the major issues encountered, how they were investigated, and the solutions that were implemented. Recording these experiences provides valuable insight into the practical aspects of deploying and maintaining enterprise security infrastructure.

---

# Issue 1: Ubuntu Storage Partitioning

## Problem

During the initial Ubuntu Server installation, an unsuitable storage layout was selected.

Although Ubuntu installed successfully, the issue became apparent during the Wazuh installation when disk space was exhausted, preventing components from being installed correctly.

---

## Investigation

Disk usage was monitored using:

```bash
df -h
```

This command displayed the available and used space on all mounted file systems.

The output confirmed that the root partition did not have sufficient free space for the Wazuh installation.

---

## Resolution

Rather than attempting to resize partitions after installation, the Ubuntu virtual machine was rebuilt using a larger virtual disk and an improved storage layout.

---

## Lesson Learned

Planning storage requirements before deploying enterprise software avoids unnecessary rebuilding and simplifies future maintenance.

---

# Issue 2: VirtualBox Network Configuration

## Problem

Initially, the Ubuntu virtual machine used a NAT network adapter.

While the server had internet access, the Wazuh Dashboard was difficult to access directly from the host machine.

---

## Investigation

Network connectivity was verified using:

```bash
hostname -I
```

```bash
ping
```

The server could communicate externally, but direct access from the host machine was limited.

---

## Resolution

The network adapter was changed from:

```
NAT
```

to

```
Bridged Adapter
```

The virtual machine then obtained its own IP address on the local network, allowing the Wazuh Dashboard to be accessed directly through a web browser.

---

## Lesson Learned

Different VirtualBox networking modes serve different purposes.

- NAT is ideal for providing internet access.
- Bridged networking allows the virtual machine to appear as an independent device on the network, making communication between systems much simpler.

---

# Issue 3: Missing `wazuh-install-files.tar`

## Problem

During the first Wazuh installation, the assisted installer completed with errors and failed to generate:

```
wazuh-install-files.tar
```

This archive normally contains installation assets and certificates generated during deployment.

---

## Investigation

Installation logs were reviewed and several verification commands were executed to determine which components had been installed successfully.

The missing archive indicated that the installation had terminated before completing all deployment stages.

---

## Resolution

Rather than attempting to manually recreate the missing files, the installation was restarted after rebuilding the Ubuntu virtual machine.

The second installation generated the archive successfully.

---

## Lesson Learned

Missing installation artefacts often indicate an incomplete installation rather than a single missing file.

---

# Issue 4: Wazuh Manager Failed to Start

## Problem

Although portions of Wazuh appeared to install correctly, the Wazuh Manager service could not be started.

The expected management utility:

```
/var/ossec/bin/wazuh-control
```

was missing.

---

## Investigation

The following commands were used:

```bash
sudo systemctl status wazuh-manager
```

```bash
sudo ss -tulpn | grep wazuh
```

The service status confirmed that the manager was not running correctly, while socket inspection showed that only some Wazuh services were listening.

---

## Resolution

Multiple repair attempts were made, but the installation remained inconsistent.

A clean rebuild was ultimately chosen.

---

## Lesson Learned

Sometimes rebuilding a failed deployment is significantly faster and more reliable than attempting to repair partially installed software.

---

# Issue 5: SSH Authentication Problems

## Problem

SSH connections to the Windows Server endpoint failed despite using the correct password.

---

## Investigation

Several checks were performed, including:

```powershell
whoami
```

```powershell
Get-Service sshd
```

Network connectivity between the Windows Server and Ubuntu Server was also verified.

The issue was narrowed down through systematic verification rather than assuming an incorrect password.

---

## Resolution

The SSH service configuration and authentication settings were reviewed until remote access functioned correctly.

---

## Lesson Learned

Authentication issues should be investigated methodically by verifying user accounts, services, firewall configuration, and network connectivity before assuming credential problems.

---

# Issue 6: VirtualBox Clipboard Sharing

## Problem

Copying commands between the Windows virtual machine and the host operating system was initially unavailable.

This significantly slowed configuration and troubleshooting.

---

## Investigation

VirtualBox settings were reviewed and Guest Additions were found to be missing.

---

## Resolution

Oracle VirtualBox Guest Additions were installed.

After rebooting the virtual machine, the following options were enabled:

- Shared Clipboard → Bidirectional
- Drag and Drop → Bidirectional

Clipboard functionality worked correctly after these changes.

---

## Lesson Learned

Installing Guest Additions immediately after deploying a virtual machine greatly improves usability and productivity.

---

# Issue 7: Windows Server Installation

## Problem

The initial installation used:

```
Windows Server 2022 Standard Evaluation
```

which installs Server Core.

This version does not include the graphical desktop environment.

---

## Resolution

The operating system was reinstalled using:

```
Windows Server 2022 Standard Evaluation (Desktop Experience)
```

---

## Lesson Learned

Understanding the differences between Server Core and Desktop Experience helps ensure the correct edition is selected for the intended use case.

---

# Issue 8: Wazuh Agent Connectivity

## Problem

After installing the Windows Wazuh agent, the endpoint did not immediately appear in the Wazuh Dashboard.

---

## Investigation

Several diagnostic steps were performed:

### Verify the agent service

```powershell
Get-Service Wazuh
```

### Verify the agent configuration

```powershell
Get-Content "C:\Program Files (x86)\ossec-agent\ossec.conf"
```

### Verify the server IP

```bash
hostname -I
```

### Verify communication ports

```bash
sudo ss -tulpn | grep wazuh
```

The investigation revealed that the server address in the agent configuration had been entered incorrectly (`198.168.100.15` instead of `192.168.100.15`).

---

## Resolution

The server IP address in `ossec.conf` was corrected and the agent service was restarted.

The Windows endpoint successfully enrolled and began forwarding logs to Wazuh.

---

## Lesson Learned

Even a single incorrect digit in an IP address can prevent endpoint enrolment. Always verify network configuration before troubleshooting more complex causes.

---

# Key Troubleshooting Techniques Used

Throughout the project, several common troubleshooting principles proved invaluable:

- Verify before assuming.
- Read service status outputs carefully.
- Check log files and error messages.
- Confirm network connectivity.
- Validate IP addresses and ports.
- Troubleshoot one issue at a time.
- Rebuild when recovery becomes more time-consuming than redeployment.

---

# Skills Developed

During troubleshooting I strengthened my understanding of:

- Linux service management (`systemctl`)
- Network troubleshooting
- VirtualBox networking
- SSH configuration
- Windows Server administration
- Wazuh diagnostics
- Log interpretation
- System verification
- Root cause analysis
- Infrastructure recovery

---

# Overall Reflection

The challenges encountered while building this SOC lab were as valuable as the successful deployments themselves. Each issue required a structured approach involving observation, hypothesis, testing, and verification before implementing a solution.

Rather than viewing troubleshooting as a setback, it became an opportunity to deepen my understanding of Linux administration, Windows Server management, networking, virtualization, and SIEM deployment. These experiences reinforced the importance of patience, systematic problem-solving, and thorough documentation when working with enterprise security infrastructure.
