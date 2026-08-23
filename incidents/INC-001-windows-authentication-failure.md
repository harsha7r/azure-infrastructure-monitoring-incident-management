# INC-001 — Windows Authentication Failure

## Incident Summary

| Field | Details |
|---|---|
| Incident ID | `INC-001` |
| Incident Type | Windows Authentication Failure |
| Affected Resource | `vm-win-srv01` |
| Operating System | Windows Server |
| Event ID | `4625` |
| Severity | Warning |
| Detection Method | Azure Monitor / Log Analytics |
| Status | Resolved |
| Environment | Azure Lab |

---

## 1. Incident Description

A controlled failed authentication attempt was generated against the Windows virtual machine:

```text
vm-win-srv01
````

The Windows Security Event Log generated Event ID `4625`, indicating that an account failed to log on.

The event was successfully collected by Azure Monitor Agent and sent to the Log Analytics workspace:

```text
law-az-ops-lab
```

The event was then investigated using Kusto Query Language (KQL).

---

## 2. Detection

The incident was detected through Windows Security Event ID `4625`.

The monitoring flow was:

```text
Windows Authentication Failure
            ↓
Windows Event ID 4625
            ↓
Azure Monitor Agent
            ↓
Data Collection Rule
            ↓
Log Analytics
            ↓
KQL Investigation
            ↓
Azure Monitor Alert
            ↓
Email Notification
```

---

## 3. Affected Resource

The affected Azure virtual machine was:

```text
vm-win-srv01
```

The event was recorded in the Windows:

```text
Security
```

event log.

---

## 4. Event Details

### Event ID

```text
4625
```

### Event Description

Event ID `4625` represents a failed Windows logon attempt.

The investigation identified authentication information including:

* Target username
* Target domain
* Logon type
* Status
* Substatus
* Source IP address
* Workstation name

---

## 5. Investigation

The following KQL query was used to identify failed authentication events:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
| summarize FailedLogins = count() by bin(TimeGenerated, 5m)
| sort by TimeGenerated desc
```

A detailed investigation query was also used to extract authentication information from the Windows event data.

The detailed query is available at:

```text
kql/failed-login-investigation.kql
```

---

## 6. Investigation Findings

The investigation identified the following information during the controlled test:

| Field                 | Finding        |
| --------------------- | -------------- |
| Computer              | `vm-win-srv01` |
| Event ID              | `4625`         |
| Event Log             | `Security`     |
| Account               | `azureadmin`   |
| Logon Type            | `3`            |
| Status                | `0xc000006d`   |
| SubStatus             | `0xc000006a`   |
| Authentication Result | Failed         |

The event indicated an authentication failure caused by invalid credentials during the controlled test.

---

## 7. Root Cause

The root cause of the incident was incorrect authentication credentials used during the controlled laboratory test.

The failure was intentional and was generated to validate the monitoring and alerting workflow.

---

## 8. Corrective Action

The correct credentials were used to authenticate to the Windows virtual machine.

The VM was subsequently accessed successfully.

No changes to the Azure infrastructure were required.

---

## 9. Validation

The following validation steps were completed:

- Windows Event ID `4625` was generated.
- Azure Monitor Agent collected the event.
- Data Collection Rule processed the event.
- Event appeared in Log Analytics.
- KQL successfully identified the event.
- Authentication details were extracted.
- Azure Monitor alert detected the configured condition.
- Action Group sent an email notification.
- Alert entered the `Fired` state.
- Successful authentication was confirmed after corrective action.

---

## 10. Monitoring Alert

The Azure Monitor alert used for this incident was:

```text
Windows Failed Login - vm-win-srv01
```

### Alert Configuration

| Setting              | Value                          |
| -------------------- | ------------------------------ |
| Signal Type          | Log Search                     |
| Workspace            | `law-az-ops-lab`               |
| Event ID             | `4625`                         |
| Measurement          | Table rows                     |
| Aggregation          | Count                          |
| Evaluation Window    | 5 minutes                      |
| Threshold            | ≥ 1                            |
| Evaluation Frequency | 5 minutes                      |
| Severity             | Warning                        |
| Action Group         | `VMI-ActionGroup-vm-win-srv01` |
| Notification         | Email                          |
| Status               | Enabled                        |

---

## 11. Notification

The Azure Monitor Action Group sent an email notification when the alert condition was met.

The notification confirmed:

```text
Alert Name:
Windows Failed Login - vm-win-srv01
```

The alert was successfully received through email.

---

## 12. Incident Timeline

```text
1. Controlled failed authentication generated
                ↓
2. Windows Event ID 4625 created
                ↓
3. Azure Monitor Agent collected event
                ↓
4. Data Collection Rule processed event
                ↓
5. Event stored in Log Analytics
                ↓
6. KQL identified failed authentication
                ↓
7. Azure Monitor alert triggered
                ↓
8. Action Group sent email notification
                ↓
9. Authentication issue investigated
                ↓
10. Correct credentials used
                ↓
11. Successful authentication confirmed
                ↓
12. Incident resolved
```

---

## 13. Evidence

The following evidence can be included in the project:

* Windows Event ID 4625
* Log Analytics KQL results
* Failed login investigation results
* Azure Monitor alert configuration
* Azure Monitor alert history
* Email notification
* Successful authentication validation

Screenshots should be stored in:

```text
screenshots/
```

## 14. Lessons Learned

This incident demonstrated the importance of:

* Centralized Windows event logging
* Azure Monitor Agent
* Data Collection Rules
* Log Analytics
* KQL-based investigation
* Automated alerting
* Email notifications
* Structured incident management
* Root-cause analysis
* Evidence-based troubleshooting

---

## 15. Final Status

**Resolved**

The monitoring and alerting workflow successfully detected and notified on the controlled Windows authentication failure.

The incident was investigated, the root cause was identified, corrective action was completed, and successful authentication was validated.

---

## Related Documentation

* [Project README](../README.md)
* [Azure Architecture](../architecture/architecture.md)
* [Heartbeat KQL](../kql/heartbeat.kql)
* [Failed Login Count KQL](../kql/failed-login-count.kql)
* [Failed Login Investigation KQL](../kql/failed-login-investigation.kql)

