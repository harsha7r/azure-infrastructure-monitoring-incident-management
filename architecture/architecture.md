## Azure Infrastructure Architecture

## Overview

This project implements a small Azure infrastructure lab designed to demonstrate cloud infrastructure administration, monitoring, logging, alerting, and incident management.

## Azure Environment

The environment is deployed in the `Australia East` Azure region.

### Resource Group

- Resource Group: `rg-az-ops-lab`
- Project tag: `azure-operations-lab`
- Environment tag: `lab`

### Virtual Machines

| VM | Operating System | Purpose |
|---|---|---|
| `vm-win-srv01` | Windows Server 2022 | Windows administration, monitoring and incident testing |
| `vm-linux-srv01` | Ubuntu Server 24.04 LTS | Linux administration and web service monitoring |

## Networking

The environment uses:

- Azure Virtual Network
- Windows subnet
- Linux subnet
- Network Security Groups
- Public IP addresses for lab connectivity

## Monitoring Architecture

The Windows VM uses Azure Monitor Agent for telemetry collection.

Windows Event Logs are collected using a Data Collection Rule and sent to Log Analytics.

```text
                    Azure Infrastructure
                           |
                    rg-az-ops-lab
                           |
                    vnet-az-ops-lab
                     /             \
                    /               \
             Windows Subnet      Linux Subnet
                  |                   |
           vm-win-srv01        vm-linux-srv01
                  |
        Azure Monitor Agent
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

## Windows Monitoring

The Windows VM uses:

* Azure Monitor Agent
* Windows Event Logs
* Data Collection Rules
* Log Analytics
* KQL
* Azure Monitor Alerts
* Action Groups

## Incident Monitoring Flow

```text
Incident
   ↓
Event Detection
   ↓
Log Collection
   ↓
Log Analytics
   ↓
KQL Investigation
   ↓
Root Cause Analysis
   ↓
Corrective Action
   ↓
Validation
   ↓
Alert / Notification
   ↓
Incident Documentation
```

## Security Monitoring Example

Windows Security Event ID `4625` is monitored to identify failed authentication attempts.

The project includes an Azure Monitor alert that can notify administrators when the configured number of failed authentication events is detected within the configured time window.

## Project Goal

The architecture demonstrates an end-to-end Azure operations workflow combining:

* Infrastructure administration
* Cloud networking
* Monitoring
* Logging
* KQL
* Alerting
* Incident management
* Root-cause analysis
* Technical documentation



