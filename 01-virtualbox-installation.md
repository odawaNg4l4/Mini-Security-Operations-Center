# Oracle VirtualBox Lab Setup

## Project Overview

The first stage of building my Security Operations Center (SOC) laboratory was creating a virtualized environment using Oracle VirtualBox. Virtualization allows multiple operating systems to run simultaneously on a single physical computer while remaining isolated from one another.

This environment serves as the foundation for the remainder of the SOC project, enabling endpoint monitoring, attack simulation, log collection, and incident detection without affecting the host operating system.

---

# Objectives

The objectives of this stage were to:

- Install Oracle VirtualBox.
- Create a virtual lab environment.
- Configure virtual networking.
- Deploy multiple virtual machines.
- Prepare the infrastructure for SIEM deployment.
- Build a safe environment for cybersecurity testing.

---

# Lab Architecture

```
                        Host Computer
                              │
                    Oracle VirtualBox
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        │                     │                     │
 Ubuntu Server          Windows Server         Kali Linux
    (Wazuh)               Endpoint           Attack Machine
        │                     │                     │
        └─────────────────────┼─────────────────────┘
                    Internal Virtual Network
```

---

# Virtual Machines

The laboratory consists of three virtual machines.

| Machine | Purpose |
|----------|---------|
| Ubuntu Server | Hosts the Wazuh SIEM platform |
| Windows Server 2022 | Simulates an enterprise Windows endpoint |
| Kali Linux | Used for penetration testing, attack simulation and security assessment |

---

# Resource Allocation

Each virtual machine was configured with similar hardware specifications.

| Resource | Allocation |
|----------|------------|
| CPU | 2 vCPUs |
| Memory | 4 GB RAM |
| Storage | 48–50 GB Virtual Disk |
| Network | Internal Network |

The resource allocation provides sufficient performance while allowing all virtual machines to operate simultaneously.

---

# Network Configuration

The virtual machines were connected using VirtualBox networking to allow communication between the SIEM server and monitored endpoints.

The network configuration enables:

- SSH access
- Endpoint enrolment
- Log forwarding
- Attack simulation
- Network communication between virtual machines

---

# Why Oracle VirtualBox?

Oracle VirtualBox was selected because it provides:

- Free and open-source virtualization
- Snapshot support
- Easy virtual networking
- Cross-platform compatibility
- Support for multiple operating systems
- Safe isolation from the host operating system

---

# Challenges Encountered

## Resource Planning

Running three virtual machines simultaneously required careful allocation of CPU and RAM to prevent exhausting host resources.

### Resolution

Each virtual machine was assigned two virtual CPUs and approximately 4 GB of RAM after evaluating host performance.

---

## Clipboard Sharing

Copying commands between the host operating system and guest operating systems was initially unavailable.

### Resolution

VirtualBox Guest Additions were installed where supported, followed by enabling:

- Shared Clipboard → Bidirectional
- Drag and Drop → Bidirectional

This significantly improved workflow efficiency when transferring commands and configuration files.

---

## Network Connectivity

Proper communication between the virtual machines was essential before deploying Wazuh.

Connectivity was verified using:

- ping
- SSH
- IP address verification
- Windows networking tools

---

# Verification

The virtual lab was considered ready after confirming that:

- Oracle VirtualBox was functioning correctly.
- All virtual machines booted successfully.
- Virtual networking was operational.
- Machines could communicate with each other.
- Sufficient system resources were available.

---

# Skills Developed

During this stage I gained practical experience with:

- Virtualization
- Virtual networking
- Infrastructure planning
- Resource allocation
- Linux administration
- Windows Server administration
- Remote access
- Troubleshooting virtual environments

---

# Lessons Learned

Building the virtual infrastructure before deploying security tools mirrors real-world enterprise environments.

Proper planning of compute resources, networking, and operating system deployment reduces configuration issues later in the project and provides a stable foundation for SIEM deployment.

---

# Technologies Used

- Oracle VirtualBox
- Virtual Networking
- Ubuntu Server
- Windows Server 2022
- Kali Linux
- SSH

---

# Next Steps

With the virtual infrastructure successfully deployed, the next phase focuses on installing and configuring the operating systems before deploying Wazuh and enrolling monitored endpoints.

