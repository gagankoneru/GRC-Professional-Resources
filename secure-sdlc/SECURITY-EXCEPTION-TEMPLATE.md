# Security Exception Template

Use this template when a required Secure SDLC control or release gate cannot be satisfied before deployment.

```text
Exception ID:
Application/service:
Environment:
Control or gate not satisfied:
Related finding(s):

Risk statement:
Threat scenario:
Affected assets/data:
Exposure:
Exploitability:
Potential impact:

Reason exception is required:
Interim safeguards:
Monitoring/detection in place:

Engineering remediation:
Remediation owner:
Target remediation date:

Risk owner:
Security reviewer:
Approver:
Approval date:
Expiry date:

Validation required before closure:
Closure evidence:
```

## Decision principles

A valid exception should be:

- **specific** — tied to a known control failure or risk;
- **owned** — someone with appropriate authority accepts the residual risk;
- **temporary** — it has an expiry date;
- **mitigated** — interim safeguards are considered;
- **actionable** — permanent remediation is linked to engineering work;
- **observable** — monitoring is strengthened where practical while the risk remains open.

An exception should not be used to redefine the security baseline permanently. If the same exception recurs across multiple teams or releases, review whether the underlying control, platform capability, or engineering process needs to change.
