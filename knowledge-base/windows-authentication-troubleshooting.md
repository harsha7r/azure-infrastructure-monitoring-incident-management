# Create the Windows Troubleshooting Knowledge Base

This will show that you can **troubleshoot an issue and document the procedure**, which is valuable for a Desktop/Windows Support role.

## Windows Authentication Troubleshooting

## Purpose

This knowledge-base article provides a structured troubleshooting procedure for investigating failed Windows authentication attempts on an Azure virtual machine.

The procedure uses Windows Event Logs, Azure Monitor Agent, Data Collection Rules, Log Analytics, and KQL.

---

## Symptoms

Common symptoms may include:

- User unable to log in to the Windows VM
- RDP authentication failure
- Repeated password prompts
- Account authentication failure
- Multiple Windows Event ID `4625` events
- Azure Monitor failed-login alert

---

## Environment

| Component | Value |
|---|---|
| Windows VM | `vm-win-srv01` |
| Operating System | Windows Server |
| Monitoring Agent | Azure Monitor Agent |
| Log Analytics | `law-az-ops-lab` |
| Data Collection Rule | `dcr-windows-events` |
| Event ID | `4625` |

---

## 1. Initial Checks

When a Windows authentication failure is reported, perform the following checks:

1. Confirm that the VM is running.
2. Confirm network connectivity.
3. Verify the username.
4. Verify the credentials.
5. Check whether the account is locked.
6. Check Windows Security Event Logs.
7. Search Log Analytics for Event ID `4625`.
8. Review the authentication details.
9. Check the source IP address.
10. Determine whether the failure is isolated or repeated.

---

## 2. Check Windows Event Logs

On the Windows VM, open:

```text
Event Viewer
    ↓
Windows Logs
    ↓
Security
````

Search for:

```text
Event ID: 4625
```

Event ID `4625` indicates a failed Windows logon attempt.

---

## 3. Check Using PowerShell

The following PowerShell command can be used to find recent failed authentication events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = "Security"
    Id = 4625
} -MaxEvents 20
```

---

## 4. Check Log Analytics

Open:

```text
Azure Portal
    ↓
Log Analytics Workspace
    ↓
law-az-ops-lab
    ↓
Logs
```

Run:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
| sort by TimeGenerated desc
```

---

## 5. Count Failed Logins

To identify repeated authentication failures:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
| summarize FailedLogins = count() by bin(TimeGenerated, 5m)
| sort by TimeGenerated desc
```

This groups failed login events into five-minute intervals.

---

## 6. Detailed Investigation

Use the following query to extract authentication information:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
| extend
    TargetUserName = extract(@"<Data Name=""TargetUserName"">(.*?)</Data>", 1, EventData),
    TargetDomainName = extract(@"<Data Name=""TargetDomainName"">(.*?)</Data>", 1, EventData),
    LogonType = extract(@"<Data Name=""LogonType"">(.*?)</Data>", 1, EventData),
    Status = extract(@"<Data Name=""Status"">(.*?)</Data>", 1, EventData),
    SubStatus = extract(@"<Data Name=""SubStatus"">(.*?)</Data>", 1, EventData),
    IpAddress = extract(@"<Data Name=""IpAddress"">(.*?)</Data>", 1, EventData),
    WorkstationName = extract(@"<Data Name=""WorkstationName"">(.*?)</Data>", 1, EventData)
| project
    TimeGenerated,
    Computer,
    TargetUserName,
    TargetDomainName,
    LogonType,
    Status,
    SubStatus,
    IpAddress,
    WorkstationName
| sort by TimeGenerated desc
```

---

## 7. Important Investigation Fields

| Field              | Purpose                                          |
| ------------------ | ------------------------------------------------ |
| `TargetUserName`   | Account involved in the authentication attempt   |
| `TargetDomainName` | Domain associated with the account               |
| `LogonType`        | Type of authentication attempt                   |
| `Status`           | Primary authentication failure status            |
| `SubStatus`        | More specific authentication failure information |
| `IpAddress`        | Source IP address                                |
| `WorkstationName`  | Workstation involved in the authentication       |

---

## 8. Common Logon Types

Some common Windows logon types include:

| Logon Type | Description              |
| ---------- | ------------------------ |
| `2`        | Interactive logon        |
| `3`        | Network logon            |
| `4`        | Batch logon              |
| `5`        | Service logon            |
| `7`        | Unlock                   |
| `8`        | NetworkCleartext         |
| `10`       | Remote Interactive / RDP |

---

## 9. Authentication Status Codes

Example authentication status codes may include:

| Status       | Meaning                                    |
| ------------ | ------------------------------------------ |
| `0xc000006d` | Bad username or authentication information |
| `0xc000006a` | Incorrect password                         |

The exact interpretation should always be validated against the complete Windows event and the surrounding authentication context.

---

## 10. Root Cause Analysis

Possible causes of Event ID `4625` include:

* Incorrect password
* Incorrect username
* Expired credentials
* Locked account
* Service using outdated credentials
* Scheduled task using outdated credentials
* Incorrect network authentication
* Misconfigured application
* Automated authentication attempts
* Potential brute-force activity

---

## 11. Corrective Actions

Depending on the root cause:

### Incorrect Credentials

Verify the username and password and retry authentication using approved credentials.

### Account Lockout

Check the account status and follow the organization's account-unlock procedure.

### Service or Scheduled Task

Check whether a Windows service or scheduled task is using outdated credentials.

### Suspicious Authentication Activity

Investigate the source IP address and authentication pattern.

Escalate according to the organization's security incident-response process.

---

## 12. Azure Monitor Alert

The project uses:

```text
Windows Failed Login - vm-win-srv01
```

The alert monitors Event ID `4625` and sends an email notification through the configured Action Group.

---

## 13. Troubleshooting Workflow

```text
User Reports Login Failure
          ↓
Check VM Availability
          ↓
Check Network Connectivity
          ↓
Verify Credentials
          ↓
Check Event ID 4625
          ↓
Query Log Analytics
          ↓
Investigate Username / Logon Type / IP
          ↓
Determine Root Cause
          ↓
Apply Corrective Action
          ↓
Test Authentication
          ↓
Verify Monitoring
          ↓
Document Resolution
```

---

## 14. Validation

After corrective action:

* Confirm successful authentication.
* Confirm the VM is accessible.
* Confirm no unexpected repeated Event ID `4625` events.
* Confirm Azure Monitor Agent is healthy.
* Confirm the Data Collection Rule is functioning.
* Confirm Log Analytics continues receiving telemetry.
* Confirm the alert configuration remains enabled.

---

## 15. Security Considerations

Authentication failures should be investigated carefully when they are:

* Repeated
* From unknown IP addresses
* Targeting privileged accounts
* Occurring outside normal working patterns
* Distributed across multiple accounts
* Associated with other suspicious events

Do not expose passwords, tokens, private keys, or other credentials during investigation or documentation.

---

## 16. Incident Documentation

When the issue is resolved, document:

* Incident ID
* Affected system
* Date and time
* Symptoms
* Event ID
* Investigation results
* Root cause
* Corrective action
* Validation results
* Evidence
* Final status

The project incident example is documented in:

```text
incidents/INC-001-windows-authentication-failure.md
```

---

## 17. Final Resolution

A Windows authentication failure is considered resolved when:

* The root cause has been identified.
* Corrective action has been completed.
* Successful authentication has been verified.
* Monitoring is functioning correctly.
* No unexpected repeated failures are observed.
* The incident has been documented.

---

## Related Documentation

* [Project README](../README.md)
* [Azure Architecture](../architecture/architecture.md)
* [Failed Login Count KQL](../kql/failed-login-count.kql)
* [Failed Login Investigation KQL](../kql/failed-login-investigation.kql)
* [INC-001 Windows Authentication Failure](../incidents/INC-001-windows-authentication-failure.md)
* [Windows Failed Login Alert](../alerts/failed-login-alert.md)

