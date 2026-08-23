
# Azure Infrastructure, Monitoring & Incident Management

## Project Overview

This project demonstrates the design, implementation, monitoring, troubleshooting, and incident management of a Microsoft Azure infrastructure environment.

The project includes Windows and Linux virtual machines, virtual networking, Network Security Groups, Azure Monitor, Azure Monitor Agent, Data Collection Rules, Log Analytics, KQL, PowerShell, Azure CLI, and incident management.

The project follows an end-to-end infrastructure operations workflow:

**Monitoring → Alert Detection → Investigation → Troubleshooting → Root Cause Analysis → Resolution → Validation → Documentation**

---

## Project Objectives

- Build and manage Azure infrastructure.
- Deploy and administer Windows and Linux virtual machines.
- Configure virtual networks and subnets.
- Configure Network Security Groups.
- Implement Azure Monitor and Azure Monitor Agent.
- Configure Data Collection Rules.
- Collect and analyse Windows Event Logs.
- Use Log Analytics for centralized log management.
- Use KQL for monitoring and investigation.
- Configure Azure Monitor alerts.
- Configure Action Groups and email notifications.
- Simulate real-world infrastructure incidents.
- Perform troubleshooting and root-cause analysis.
- Document incident resolution procedures.
- Automate routine administration using PowerShell and Azure CLI.
- Maintain technical documentation and knowledge-base articles.

---

## Technologies

- Microsoft Azure
- Azure Virtual Machines
- Azure Virtual Network
- Network Security Groups
- Azure Monitor
- Azure Monitor Agent
- Data Collection Rules
- Log Analytics
- KQL
- Windows Server
- Ubuntu Linux
- NGINX
- PowerShell
- Azure CLI
- Git
- GitHub

---

## Azure Environment

### Resource Group

| Property | Value |
|---|---|
| Resource Group | `rg-az-ops-lab` |
| Region | Australia East |
| Project Tag | `azure-operations-lab` |
| Environment Tag | `lab` |

### Virtual Machines

| Virtual Machine | Operating System | Purpose |
|---|---|---|
| `vm-win-srv01` | Windows Server 2022 | Windows administration and monitoring |
| `vm-linux-srv01` | Ubuntu Server 24.04 LTS | Linux administration and NGINX monitoring |

---

## Architecture

```text
                    Azure Subscription
                           |
                    rg-az-ops-lab
                           |
                    vnet-az-ops-lab
                     /             \
                    /               \
          snet-windows            snet-linux
                |                     |
         vm-win-srv01          vm-linux-srv01
                |                     |
     Azure Monitor Agent             NGINX
                |
     Data Collection Rule
                |
      Windows Event Logs
                |
       Log Analytics
       law-az-ops-lab
                |
               KQL
                |
       Azure Monitor Alert
                |
          Action Group
                |
        Email Notification
````

---

## Monitoring Architecture

The Windows virtual machine is monitored using Azure Monitor Agent.

Windows Event Logs are collected using a Data Collection Rule and sent to the Log Analytics workspace.

```text
vm-win-srv01
     |
     v
Azure Monitor Agent
     |
     v
Data Collection Rule
     |
     v
Windows Event Logs
     |
     v
Log Analytics
     |
     v
KQL Investigation
     |
     v
Azure Monitor Alert
     |
     v
Action Group
     |
     v
Email Notification
```

---

## Incident Management Workflow

The project follows a structured incident management process:

```text
Monitoring
    ↓
Alert Detection
    ↓
Incident Identification
    ↓
Evidence Collection
    ↓
Investigation
    ↓
Root Cause Analysis
    ↓
Corrective Action
    ↓
Validation
    ↓
Documentation
```

---

## Monitoring Use Case

### Windows Authentication Failure

The project monitors Windows Security Event ID `4625`.

Event ID `4625` represents a failed Windows logon attempt.

The monitoring workflow is:

```text
Failed Windows Login
        ↓
Windows Event ID 4625
        ↓
Azure Monitor Agent
        ↓
Data Collection Rule
        ↓
Log Analytics
        ↓
KQL
        ↓
Azure Monitor Alert
        ↓
Action Group
        ↓
Email Notification
```

---

## Azure Monitor Alert

The project includes an Azure Monitor log search alert:

**Windows Failed Login - vm-win-srv01**

The alert monitors Event ID `4625` from the Windows virtual machine.

### Alert Configuration

| Setting              | Value                                 |
| -------------------- | ------------------------------------- |
| Alert Name           | `Windows Failed Login - vm-win-srv01` |
| Signal Type          | Log Search                            |
| Workspace            | `law-az-ops-lab`                      |
| Event ID             | `4625`                                |
| Measurement          | Table rows                            |
| Aggregation          | Count                                 |
| Evaluation Window    | 5 minutes                             |
| Threshold            | ≥ 1                                   |
| Evaluation Frequency | 5 minutes                             |
| Severity             | Warning                               |
| Action Group         | `VMI-ActionGroup-vm-win-srv01`        |
| Notification         | Email                                 |
| Status               | Enabled                               |

> **Note:** The threshold of `≥ 1` is configured for this learning lab to make testing straightforward. Production environments should use an appropriate threshold and detection strategy to reduce alert noise.

---

## Incident Management Example

### INC-001 — Windows Authentication Failure

A controlled failed authentication attempt was generated against:

`vm-win-srv01`

The Windows Security Event Log generated Event ID `4625`.

The event was successfully collected by Azure Monitor Agent and stored in Log Analytics.

The event was investigated using KQL.

### Investigation

The investigation identified authentication information including:

* Account
* Logon type
* Authentication status
* Authentication substatus
* Source IP address
* Workstation information

### Root Cause

The test authentication failure was caused by incorrect credentials.

### Resolution

The correct credentials were used and successful access to the Windows VM was confirmed.

### Validation

The monitoring workflow was successfully validated:

1. Event ID `4625` was generated.
2. The event was collected by Azure Monitor Agent.
3. The event appeared in Log Analytics.
4. KQL successfully identified the event.
5. Azure Monitor detected the alert condition.
6. The Action Group sent an email notification.
7. The alert entered the `Fired` state.

---

## KQL

Kusto Query Language is used throughout the project for monitoring and investigation.

### Heartbeat Query

```kusto
Heartbeat
| where Computer contains "vm-win-srv01"
| sort by TimeGenerated desc
| take 20
```

### Failed Login Count

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
| summarize FailedLogins = count() by bin(TimeGenerated, 5m)
| sort by TimeGenerated desc
```

### Failed Login Investigation

The investigation query extracts:

* Target username
* Domain
* Logon type
* Status
* Substatus
* Source IP address
* Workstation name

Detailed KQL queries are maintained in the `kql/` directory.

---

## Repository Structure

```text
azure-infrastructure-monitoring-incident-management/
│
├── README.md
│
├── architecture/
│   └── architecture.md
│
├── azure-cli/
│   └── commands.md
│
├── powershell/
│   └── scripts.md
│
├── kql/
│   ├── heartbeat.kql
│   ├── failed-login-count.kql
│   └── failed-login-investigation.kql
│
├── incidents/
│   └── INC-001-windows-authentication-failure.md
│
├── alerts/
│   └── failed-login-alert.md
│
├── knowledge-base/
│   └── windows-authentication-troubleshooting.md
│
└── screenshots/
    ├── 01-resource-group.png
    ├── 02-windows-vm.png
    ├── 03-linux-vm.png
    ├── 04-nginx.png
    ├── 05-log-analytics.png
    ├── 06-azure-monitor-agent.png
    ├── 07-dcr.png
    ├── 08-kql-heartbeat.png
    ├── 09-kql-event-4625.png
    ├── 10-kql-investigation.png
    ├── 11-alert-rule.png
    ├── 12-alert-fired.png
    └── 13-email-notification.png
```

---

## Learning Outcomes

Through this project, I am developing practical experience in:

* Azure infrastructure administration
* Windows Server administration
* Linux administration
* Cloud networking
* Network Security Groups
* Azure Monitor
* Azure Monitor Agent
* Data Collection Rules
* Log Analytics
* KQL
* Windows Event Logs
* Authentication troubleshooting
* Incident management
* Root-cause analysis
* Alert management
* PowerShell
* Azure CLI
* Technical documentation
* Git and GitHub

---

## Future Enhancements

Planned enhancements include:

* Linux VM monitoring
* NGINX availability monitoring
* CPU utilisation monitoring
* Network connectivity incident simulation
* Windows service failure incident
* SQL-based incident analysis
* PowerShell automation
* Azure CLI automation
* Additional KQL queries
* Additional knowledge-base articles
* Additional incident scenarios

---

## Disclaimer

This is a personal Azure lab and portfolio project created for learning and demonstrating practical technical skills.

No production systems or confidential data are used.

All incidents described in this repository are controlled laboratory tests.

---

