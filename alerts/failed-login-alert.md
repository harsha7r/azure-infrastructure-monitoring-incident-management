# Windows Failed Login Alert

## Overview

This document describes the Azure Monitor alert configured to detect failed Windows authentication attempts on the virtual machine `vm-win-srv01`.

The alert monitors Windows Security Event ID `4625` in the Log Analytics workspace and sends an email notification when the configured threshold is reached.

---

## Alert Details

| Property | Value |
|---|---|
| Alert Name | `Windows Failed Login - vm-win-srv01` |
| Alert Type | Log Search Alert |
| Severity | `2 - Warning` |
| Status | Enabled |
| Target Resource | `law-az-ops-lab` |
| Target Resource Type | Log Analytics Workspace |
| Virtual Machine | `vm-win-srv01` |
| Event ID | `4625` |
| Measurement | Table rows |
| Aggregation Type | Count |
| Aggregation Granularity | 5 minutes |
| Threshold | Greater than or equal to 1 |
| Evaluation Frequency | 5 minutes |
| Action Group | `VMI-ActionGroup-vm-win-srv01` |
| Notification | Email |

---

## Purpose

The purpose of this alert is to detect failed authentication attempts on the Windows virtual machine.

Windows Event ID `4625` is generated when an account fails to log on.

The alert provides an automated notification mechanism so that authentication failures can be investigated promptly.

---

## Monitoring Flow

```text
vm-win-srv01
      |
      v
Windows Security Event
      |
      v
Event ID 4625
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
KQL Alert Query
      |
      v
Azure Monitor Alert
      |
      v
Action Group
      |
      v
Email Notification
````

---

## Alert Query

The alert uses the following KQL query:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
```

The query filters the Windows Event table for:

* Computer: `vm-win-srv01`
* Event ID: `4625`

---

## Alert Logic

The alert uses:

**Measurement**

```text
Table rows
```

**Aggregation**

```text
Count
```

**Aggregation Granularity**

```text
5 minutes
```

**Threshold**

```text
Greater than or equal to 1
```

**Evaluation Frequency**

```text
5 minutes
```

Therefore, when at least one Event ID `4625` is detected within the configured evaluation period, the alert can fire.

---

## Action Group

The alert is connected to:

```text
VMI-ActionGroup-vm-win-srv01
```

The Action Group contains an email notification.

When the alert condition is met, Azure Monitor sends an email notification.

---

## Email Notification

The email notification confirms that the alert condition was detected.

Example subject:

```text
Azure alerts for 1 failed log in attempts in 5 min.
```

The notification contains information including:

* Alert name
* Severity
* Monitor condition
* Affected resource
* Resource type
* Time of the alert

---

## Testing

The alert was tested by generating a controlled failed authentication attempt against:

```text
vm-win-srv01
```

Windows generated Event ID:

```text
4625
```

The event was successfully collected and appeared in Log Analytics.

The Azure Monitor alert subsequently entered the:

```text
Fired
```

state.

An email notification was received confirming that the alerting workflow was functioning correctly.

---

## Validation Checklist

* [x] Event ID `4625` generated
* [x] Event collected by Azure Monitor Agent
* [x] Event delivered to Log Analytics
* [x] KQL query returned the event
* [x] Alert rule created
* [x] Alert rule enabled
* [x] Alert condition configured
* [x] Action Group attached
* [x] Email notification configured
* [x] Alert successfully fired
* [x] Email notification received

---

## Troubleshooting

If the alert does not fire, check the following:

### 1. Verify the VM is running

Confirm:

```text
vm-win-srv01
```

is running.

### 2. Verify Azure Monitor Agent

Check that the Azure Monitor Agent extension is installed and provisioning successfully.

### 3. Verify the Data Collection Rule

Confirm that:

```text
dcr-windows-events
```

is associated with the Windows VM.

### 4. Verify Log Analytics

Check that Event ID `4625` appears in:

```text
law-az-ops-lab
```

using KQL.

### 5. Verify the alert query

Run:

```kusto
Event
| where Computer == "vm-win-srv01"
| where EventID == 4625
```

### 6. Verify the Action Group

Confirm:

```text
VMI-ActionGroup-vm-win-srv01
```

is attached to the alert rule and contains the correct email notification.

---

## Security Considerations

Failed authentication alerts can help identify:

* Incorrect credentials
* User mistakes
* Misconfigured services
* Automated authentication failures
* Potential brute-force activity
* Suspicious login attempts

In a production environment, alert thresholds should be tuned to reduce false positives and alert fatigue.

Additional investigation should consider:

* Source IP address
* Username
* Logon type
* Frequency of failures
* Geographic origin
* Related security events
* Successful logons following failures

---

## Related Documentation

* [Project README](../README.md)
* [Azure Architecture](../architecture/architecture.md)
* [Failed Login Count KQL](../kql/failed-login-count.kql)
* [Failed Login Investigation KQL](../kql/failed-login-investigation.kql)
* [Incident INC-001](../incidents/INC-001-windows-authentication-failure.md)

---

## Status

**Enabled and successfully tested**

