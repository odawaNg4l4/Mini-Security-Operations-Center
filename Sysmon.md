# Installing and Integrating Sysmon with Wazuh

## Event Flow

```text
Notepad Starts
        │
        ▼
Sysmon Records the Event
        │
        ▼
Windows Event Log
(Sysmon Operational)
        │
        ▼
Wazuh Agent Reads the Event
        │
        ▼
Wazuh Manager
        │
        ▼
Wazuh Indexer
        │
        ▼
Wazuh Dashboard
```

---

# Objective

The objective of this lab was to install **Microsoft Sysmon** on the Windows Server endpoint and configure the **Wazuh Agent** to collect Sysmon events.

Sysmon provides detailed visibility into system activity, including:

- Process creation
- Network connections
- Registry modifications
- Driver loading
- File creation
- Image loading
- Process termination

Once integrated with Wazuh, these events can be used to:

- Detect suspicious behaviour
- Perform threat hunting
- Investigate security incidents
- Improve endpoint visibility

---

# Lab Environment

| Machine | Operating System | Role |
|---------|------------------|------|
| Ubuntu Server | Ubuntu Server | Wazuh Manager |
| Windows Server | Windows Server 2022 | Wazuh Agent + Sysmon |
| Kali Linux | Kali Linux | Attacker & Wazuh Agent |

---

# Prerequisites

Before beginning this lab, ensure that:

- Oracle VirtualBox is installed.
- Ubuntu Server is running the Wazuh platform successfully.
- Windows Server has already been enrolled as a Wazuh Agent.
- Kali Linux has been enrolled as a Wazuh Agent.
- The Wazuh Dashboard is accessible through a web browser.

---

# Step 1 – Download Sysmon

Open **Microsoft Edge** on the Windows Server and navigate to the official Microsoft Sysinternals download page.

Download:

```text
Sysmon.zip
```

Extract the archive into a folder, for example:

```text
Documents\Sysmon
```

## Explanation

The extracted folder contains:

- `Sysmon64.exe`
- `Sysmon.exe`
- `Eula.txt`
- Supporting files

Since Windows Server 2022 is a 64-bit operating system, the installation uses:

```text
Sysmon64.exe
```

---

# Step 2 – Download a Sysmon Configuration

Sysmon requires a configuration file that specifies which events should be logged.

Download the **SwiftOnSecurity Sysmon configuration**:

```text
sysmonconfig-export.xml
```

Save the file inside the Sysmon folder.

Example:

```text
Documents
└── Sysmon
      ├── Sysmon64.exe
      ├── Sysmon.exe
      ├── Eula.txt
      └── sysmonconfig-export.xml
```

## Explanation

Without a configuration file, Sysmon records only minimal information.

The SwiftOnSecurity configuration is widely used because it captures valuable security events while reducing unnecessary noise.

---

# Step 3 – Install Sysmon

Open **PowerShell as Administrator**.

Navigate to the Sysmon directory.

Example:

```powershell
cd "C:\Users\Administrator\Documents\Sysmon"
```

Install Sysmon:

```powershell
.\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

## Command Explanation

| Option | Description |
|---------|-------------|
| `-accepteula` | Automatically accepts Microsoft's licence agreement |
| `-i` | Installs Sysmon |
| `sysmonconfig-export.xml` | Applies the downloaded monitoring configuration |

---

# Step 4 – Verify the Installation

Verify that the Sysmon service has been installed successfully:

```powershell
Get-Service Sysmon64
```

### Expected Output

```text
Status : Running
```

## Explanation

This confirms that Sysmon is installed and actively monitoring the endpoint.

---

# Step 5 – Verify Sysmon Event Logging

Generate a process creation event by launching Notepad.

```powershell
notepad.exe
```

Open **Event Viewer** and navigate to:

```text
Applications and Services Logs
    └── Microsoft
         └── Windows
              └── Sysmon
                   └── Operational
```

You should observe an **Event ID 1** indicating that a new process was created.

## Explanation

Event ID 1 records detailed process creation information, including:

- Process name
- Command line
- Parent process
- Process GUID
- User account
- Hash values

These details are valuable during forensic investigations and threat hunting.

---

# Step 6 – Configure the Wazuh Agent

Open the Wazuh Agent configuration file:

```powershell
notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"
```

Locate the existing `<localfile>` entries.

Add the following configuration:

```xml
<localfile>
    <location>Microsoft-Windows-Sysmon/Operational</location>
    <log_format>eventchannel</log_format>
</localfile>
```

## Explanation

This configuration instructs the Wazuh Agent to monitor the **Sysmon Operational Event Log** and forward collected events to the Wazuh Manager.

---

# Step 7 – Restart the Wazuh Agent

Restart the Wazuh Agent service to apply the updated configuration.

```powershell
Restart-Service Wazuh
```

Verify that the service is running:

```powershell
Get-Service Wazuh
```

### Expected Output

```text
Status : Running
```

## Explanation

Restarting the service reloads the updated configuration, enabling the Wazuh Agent to begin collecting Sysmon events.

---

# Step 8 – Verify in the Wazuh Dashboard

Open the **Wazuh Dashboard**.

Navigate to:

```text
Threat Hunting
        │
        ▼
Events
```

Initially, only events from the Wazuh Manager (`ubuntuserver`) were displayed.

Further investigation revealed that the following dashboard filter was enabled:

```text
manager.name: ubuntuserver
```

This filter restricted the displayed events to those generated by the Wazuh Manager only.

After removing the filter, events from enrolled agents, including the Windows Server, became visible.

## Explanation

Dashboard filters can prevent expected events from appearing even when data is being collected successfully.

When troubleshooting missing events, always verify any active filters before investigating more complex causes.

---

# Troubleshooting Performed

| Issue | Resolution |
|------|------------|
| Incorrect manager IP address in `ossec.conf` | Corrected the IP address and restarted the Wazuh Agent |
| Windows Agent not appearing initially | Verified agent enrollment and service status |
| Sysmon installation verification | Confirmed using `Get-Service Sysmon64` |
| Dashboard displaying only manager events | Identified and removed the `manager.name: ubuntuserver` filter |

---

# Skills Demonstrated

During this phase, the following technical skills were practised:

- Microsoft Sysmon deployment
- Windows Event Log analysis
- Wazuh Agent configuration
- Event Channel monitoring
- SIEM log collection
- Endpoint telemetry integration
- Windows service management
- Security event verification
- Dashboard troubleshooting
- Threat hunting preparation

---

# Lessons Learned

This exercise highlighted several important operational and security concepts:

- Sysmon significantly enhances endpoint visibility beyond standard Windows logging.
- A SIEM is only as effective as the telemetry it receives from monitored endpoints.
- Configuration changes must be followed by a service restart before taking effect.
- Dashboard filters can create misleading troubleshooting scenarios if not checked first.
- Verifying each stage of the log collection pipeline simplifies troubleshooting and helps quickly identify failures.

---

# Current Project Status

- VirtualBox lab environment configured.
- Wazuh Manager operational on Ubuntu Server.
- Windows Server successfully enrolled as a Wazuh Agent.
- Kali Linux enrolled as a Wazuh Agent.
- Microsoft Sysmon installed and operational.
- Wazuh Agent configured to collect Sysmon events.
- Sysmon events successfully forwarded to the Wazuh Dashboard.

---

# Real-World Relevance

Microsoft Sysmon is widely deployed in enterprise environments to provide enhanced endpoint telemetry for Security Operations Centres (SOCs). When integrated with SIEM platforms such as Wazuh, Sysmon enables analysts to detect malicious processes, identify persistence mechanisms, investigate lateral movement, and perform effective threat hunting. Combining detailed endpoint telemetry with centralised log analysis forms a key component of modern detection and response capabilities.
