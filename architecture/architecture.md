# Azure Infrastructure Architecture

## 1. Overview

This document describes the architecture of the Azure infrastructure used in the Azure Infrastructure, Monitoring & Incident Management project.

The environment is designed as a learning and portfolio lab to demonstrate:

- Azure infrastructure administration
- Virtual networking
- Windows and Linux administration
- Monitoring and logging
- Security event investigation
- Alerting
- Incident management

---

## 2. Azure Environment

### Resource Group

| Property | Value |
|---|---|
| Resource Group | `rg-az-ops-lab` |
| Region | Australia East |
| Project | `azure-operations-lab` |
| Environment | `lab` |

### Virtual Network

| Property | Value |
|---|---|
| Virtual Network | `vnet-az-ops-lab` |
| Windows Subnet | `snet-windows` |
| Linux Subnet | `snet-linux` |

---

## 3. Virtual Machines

### Windows Virtual Machine

| Property | Value |
|---|---|
| Name | `vm-win-srv01` |
| Operating System | Windows Server 2022 |
| Purpose | Windows administration and monitoring |
| Monitoring | Azure Monitor Agent |
| Log Collection | Windows Event Logs |

### Linux Virtual Machine

| Property | Value |
|---|---|
| Name | `vm-linux-srv01` |
| Operating System | Ubuntu Server 24.04 LTS |
| Purpose | Linux administration and web service monitoring |
| Web Server | NGINX |

---

## 4. Network Architecture

The environment uses separate subnets for Windows and Linux workloads.

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
       Windows Server              Ubuntu
                                      |
                                    NGINX
````

---

## 5. Windows Monitoring Architecture

The Windows virtual machine is monitored using Azure Monitor Agent.

The monitoring pipeline is:

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
                         KQL
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

## 6. Azure Monitor Agent

Azure Monitor Agent is installed on:

```text
vm-win-srv01
```

The agent collects monitoring data from the Windows virtual machine.

The collected data is processed according to the configured Data Collection Rule.

---

## 7. Data Collection Rule

The Windows monitoring configuration uses a Data Collection Rule:

```text
dcr-windows-events
```

The DCR is associated with:

```text
vm-win-srv01
```

The DCR collects Windows Event Logs and sends them to the Log Analytics workspace.

---

## 8. Log Analytics

The project uses the following Log Analytics workspace:

```text
law-az-ops-lab
```

Log Analytics provides centralized storage and querying of monitoring data.

Kusto Query Language (KQL) is used to investigate the collected data.

---

## 9. Windows Security Event Monitoring

The project monitors Windows Security Event ID:

```text
4625
```

Event ID 4625 represents a failed Windows logon attempt.

The monitoring workflow is:

```text
Failed Authentication
        |
        v
Windows Event ID 4625
        |
        v
Azure Monitor Agent
        |
        v
Data Collection Rule
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
Email Notification
```

---

## 10. Alert Architecture

The project contains an Azure Monitor alert:

```text
Windows Failed Login - vm-win-srv01
```

The alert evaluates Windows Event ID 4625 events.

### Alert Configuration

| Setting              | Value                          |
| -------------------- | ------------------------------ |
| Alert Type           | Log Search Alert               |
| Workspace            | `law-az-ops-lab`               |
| Event ID             | `4625`                         |
| Aggregation          | Count                          |
| Evaluation Window    | 5 minutes                      |
| Threshold            | ≥ 1                            |
| Evaluation Frequency | 5 minutes                      |
| Severity             | Warning                        |
| Action Group         | `VMI-ActionGroup-vm-win-srv01` |
| Notification         | Email                          |

The threshold is intentionally configured for this learning lab to make testing straightforward.

---

## 11. Incident Management Architecture

The project follows a structured incident-management process.

```text
                 Monitoring
                     |
                     v
              Alert Detection
                     |
                     v
             Incident Identification
                     |
                     v
              Evidence Collection
                     |
                     v
                Investigation
                     |
                     v
             Root Cause Analysis
                     |
                     v
              Corrective Action
                     |
                     v
                 Validation
                     |
                     v
               Documentation
```

---

## 12. Incident Example

The first implemented incident is:

```text
INC-001 — Windows Authentication Failure
```

The incident involves a controlled failed authentication attempt against:

```text
vm-win-srv01
```

The failed authentication generated Windows Security Event ID `4625`.

The event was collected through Azure Monitor Agent and the Data Collection Rule and stored in Log Analytics.

---

## 13. Investigation Flow

The incident investigation follows this process:

```text
Event ID 4625
      |
      v
Identify affected VM
      |
      v
Identify account
      |
      v
Review logon type
      |
      v
Review status/substatus
      |
      v
Review source IP
      |
      v
Determine probable root cause
      |
      v
Apply corrective action
      |
      v
Validate successful authentication
```

---

## 14. Project Architecture Summary

The complete architecture combines Azure infrastructure, monitoring, logging, alerting, and incident management.

```text
                         Azure Environment
                                |
                         Resource Group
                         rg-az-ops-lab
                                |
                         Virtual Network
                         vnet-az-ops-lab
                         /              \
                        /                \
               Windows Subnet        Linux Subnet
                     |                    |
              vm-win-srv01         vm-linux-srv01
                     |                    |
            Azure Monitor Agent          NGINX
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
                     |
            Incident Management
                     |
               Root Cause Analysis
                     |
                Documentation
```

---

## 15. Design Goals

The architecture is designed to demonstrate practical IT operations skills including:

* Cloud infrastructure administration
* Windows administration
* Linux administration
* Network administration
* Monitoring
* Log management
* Security event analysis
* KQL investigation
* Alert management
* Incident response
* Root-cause analysis
* Technical documentation

---

## 16. Environment Classification

This environment is a non-production Azure learning lab.

```text
Project:      azure-operations-lab
Environment:  lab
Region:       Australia East
```

No production workloads or confidential production data are used.

