# azure-infrastructure-monitoring-incident-management
Azure infrastructure, monitoring, incident management, troubleshooting, and automation portfolio project using Microsoft Azure, Entra ID, Azure Monitor, Log Analytics, KQL, PowerShell, Azure CLI, and SQL.
Azure Infrastructure, Monitoring & Incident Management
Project Overview

This project demonstrates the design, implementation, monitoring, and troubleshooting of a Microsoft Azure infrastructure environment.

The project includes Windows and Linux virtual machines, virtual networking, network security controls, Microsoft Entra ID, Azure RBAC, Azure Monitor, Log Analytics, KQL, PowerShell, Azure CLI, and SQL.

It also simulates real-world IT support and infrastructure incidents involving authentication, network connectivity, resource utilisation, service availability, and application failures.

The project follows an end-to-end incident management workflow:

Monitoring → Alert Detection → Investigation → Troubleshooting → Root Cause Analysis → Resolution → Validation → Documentation

Project Objectives
Build and manage Azure infrastructure.
Configure virtual networks, subnets, and Network Security Groups.
Deploy and administer Windows and Linux virtual machines.
Configure Microsoft Entra ID users, groups, and role-based access control.
Implement Azure Monitor and Log Analytics.
Use KQL to investigate infrastructure and application telemetry.
Simulate and troubleshoot common IT infrastructure incidents.
Perform root-cause analysis and document incident resolutions.
Analyse incident data using SQL.
Automate routine Azure administration tasks using PowerShell and Azure CLI.
Maintain technical documentation and knowledge-base articles.
Technologies
Microsoft Azure
Azure Virtual Machines
Azure Virtual Network
Network Security Groups
Microsoft Entra ID
Azure RBAC
Azure Monitor
Log Analytics
KQL
Windows Server
Linux
PowerShell
Azure CLI
SQL
Project Status

🚧 In Progress

The project is being developed step-by-step, with implementation evidence, screenshots, scripts, incident documentation, and troubleshooting procedures added throughout the build.

Architecture

The final environment will include:

                    Microsoft Entra ID
                           |
                      Azure RBAC
                           |
                    Resource Group
                           |
                    Virtual Network
                     /           \
                    /             \
             Windows Subnet    Linux Subnet
                  |                 |
             Windows VM          Linux VM
                  \                 /
                   \               /
                    Azure Monitor
                         |
                   Log Analytics
                         |
                        KQL
                         |
                   Alert Detection
                         |
                   Incident Management
                         |
              Troubleshooting & RCA
                         |
                  SQL Analysis
                         |
                 Knowledge Base
Incident Management Workflow

The project will simulate incidents such as:

Authentication failures
Network connectivity issues
High CPU/resource utilisation
Application or service failures

Each incident will be investigated using a structured process:

Identify the issue.
Assess impact.
Collect evidence.
Analyse logs and metrics.
Identify the probable root cause.
Apply corrective action.
Validate the resolution.
Document the incident.
Record preventive or follow-up actions.
Repository Structure
├── architecture/
├── azure-cli/
├── powershell/
├── kql/
├── sql/
├── incidents/
├── knowledge-base/
└── screenshots/
Learning Outcomes

Through this project, I am developing practical experience in:

Azure infrastructure administration
Cloud networking
Identity and access management
Infrastructure monitoring
Log analysis
Incident management
Technical troubleshooting
Root-cause analysis
SQL-based data analysis
PowerShell and Azure CLI automation
Technical documentation
Disclaimer

This is a personal Azure lab and portfolio project created for learning and demonstrating practical technical skills. No production systems or confidential data are used.
