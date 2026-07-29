# Kali Linux Installation

## Project Overview

Kali Linux was deployed as the attack machine within the SOC laboratory. It is used to perform penetration testing, reconnaissance, vulnerability assessments, and attack simulations against monitored systems.

Unlike Ubuntu Server and Windows Server, Kali Linux was installed using the official Oracle VirtualBox appliance (OVA), eliminating the need for a manual operating system installation and significantly reducing setup time.

This machine will later be used to generate security events that can be detected, analysed, and investigated using Wazuh.

---

# Objectives

The objectives of this phase were to:

- Deploy Kali Linux in Oracle VirtualBox.
- Configure networking.
- Update the operating system.
- Verify system resources.
- Prepare the machine for penetration testing.
- Prepare the endpoint for future Wazuh agent installation.

---

# Software Used

| Software | Version |
|----------|---------|
| Kali Linux | Latest Stable Release |
| Oracle VirtualBox | Latest Stable Release |
| Kali VirtualBox Appliance | Official OVA Image |

---

# Why Kali Linux?

Kali Linux is one of the most widely used operating systems for cybersecurity professionals and penetration testers.

It includes hundreds of pre-installed security tools used for:

- Network reconnaissance
- Vulnerability scanning
- Web application testing
- Password auditing
- Wireless security assessments
- Digital forensics
- Exploitation
- Threat simulation

Using Kali Linux enables realistic attack scenarios that generate logs and alerts for monitoring within the Wazuh SIEM platform.

---

# Deployment Process

## 1. Download the Official VirtualBox Appliance

Instead of downloading an ISO image and performing a manual installation, the official Kali Linux VirtualBox appliance (OVA) was downloaded from the Kali Linux website.

The appliance already includes:

- A pre-installed Kali Linux operating system.
- Optimised VirtualBox drivers.
- Virtual hardware configuration.
- Guest integration tools.

Using the appliance significantly reduced setup time while ensuring compatibility with Oracle VirtualBox.

---

## 2. Import the Appliance

The appliance was imported into Oracle VirtualBox using:

```
File
    → Import Appliance
```

The downloaded `.ova` file was selected, and the default virtual machine settings were reviewed before completing the import.

---

## 3. Verify Virtual Machine Resources

Before starting the virtual machine, the allocated hardware resources were reviewed.

| Resource | Allocation |
|----------|------------|
| CPUs | 2 vCPUs |
| Memory | 4 GB RAM |
| Storage | Approximately 50 GB |
| Network | Internal Network |

These resources provide sufficient performance for penetration testing while allowing multiple virtual machines to run simultaneously.

---

## 4. Boot Kali Linux

After importing the appliance, Kali Linux booted successfully without requiring any additional installation.

The operating system was verified by logging into the desktop environment.

---

# Post-Deployment Configuration

The first task after deployment was updating the operating system.

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Command Explanation

### `sudo`

Runs the command with administrative (root) privileges.

Many system administration tasks require elevated permissions.

---

### `apt update`

Downloads the latest package information from the configured software repositories.

This updates the package index but does not install any updates.

---

### `apt upgrade -y`

Installs the latest versions of installed packages.

The `-y` option automatically confirms the installation when prompted.

Updating the operating system ensures the latest bug fixes, security patches, and tool updates are applied before performing penetration testing.

---

# System Verification

After updating the operating system, several system checks were performed.

Useful commands included:

```bash
hostname
```

Displays the system hostname.

---

```bash
hostname -I
```

Displays the assigned IP address.

This IP address is later used when communicating with the Wazuh server and other virtual machines.

---

```bash
ip a
```

Displays detailed information about network interfaces and IP addressing.

---

```bash
free -h
```

Displays installed and available system memory.

---

```bash
df -h
```

Displays available disk space.

---

```bash
lscpu
```

Displays processor information and allocated virtual CPUs.

---

# Network Verification

Before continuing, network connectivity was verified to ensure communication with the Ubuntu Server and Windows Server virtual machines.

Successful communication between the virtual machines is required for:

- Penetration testing
- Endpoint monitoring
- Log forwarding
- Attack simulation

Connectivity was verified using:

```bash
ping <target-ip>
```

---

# Challenges Encountered

## Choosing the Installation Method

Rather than performing a traditional installation from an ISO image, the official Kali VirtualBox appliance was selected.

### Resolution

The appliance simplified deployment by providing a pre-configured virtual machine that was immediately ready for use.

This reduced installation time while ensuring compatibility with Oracle VirtualBox.

---

## Operating System Updates

Immediately after deployment, package updates were installed to ensure all penetration testing tools and system packages were current.

Maintaining an updated security testing environment is considered a cybersecurity best practice.

---

## Network Verification

Network connectivity was confirmed before beginning future tasks such as endpoint enrolment and attack simulation.

Reliable communication between virtual machines is essential for realistic SOC exercises.

---

# Verification Checklist

The Kali Linux deployment was considered successful after confirming:

- Kali Linux booted successfully.
- Login completed successfully.
- Operating system fully updated.
- Network connectivity established.
- IP address assigned.
- CPU, memory, and storage verified.
- Machine ready for penetration testing.
- Machine prepared for future Wazuh agent installation.

---

# Skills Developed

During this phase I gained practical experience with:

- Kali Linux deployment
- Oracle VirtualBox appliance management
- Linux system administration
- Package management using APT
- Network verification
- Penetration testing environment preparation

---

# Lessons Learned

Using the official Kali Linux VirtualBox appliance significantly reduces deployment time while providing an environment that is already configured for VirtualBox.

Updating the operating system immediately after deployment ensures security tools remain current and minimises potential issues caused by outdated packages.

Verifying network connectivity before conducting penetration testing ensures that later attack simulations and endpoint monitoring function correctly.

---

# Technologies Used

- Kali Linux
- Oracle VirtualBox
- Kali VirtualBox Appliance (OVA)
- Linux Command Line
- APT Package Manager

---

# Next Phase

With Kali Linux successfully deployed and updated, the next phase focuses on enrolling the machine into the Wazuh SIEM platform before using it to perform reconnaissance, attack simulations, and security testing against monitored endpoints.
