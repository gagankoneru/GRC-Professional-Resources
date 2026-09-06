# PCI DSS Evidence Catalogue

A reusable catalogue for designing evidence collection around PCI DSS controls.

> Independent practitioner resource. This is not an official PCI SSC assessment template and does not replace PCI DSS, SAQs, ROC instructions, testing procedures, or assessor guidance.

## Evidence design principles

Good evidence should be:

- **authoritative** — sourced from the system that performs or records the control;
- **time-bound** — shows when the control operated;
- **scope-specific** — identifies the relevant system, environment or population;
- **repeatable** — can be collected again using the same method;
- **traceable** — links control, owner, result and remediation;
- **tamper-resistant where practical** — preferably system-generated rather than manually recreated.

## Evidence catalogue

| Control domain | Example evidence | Typical source | Collection mode | Common failure mode |
| --- | --- | --- | --- | --- |
| Scope | CDE inventory, system classification, data-flow map | CMDB/cloud inventory/architecture repo | Automated + reviewed | Unknown assets or stale diagrams |
| Network controls | Firewall/security-group rules, review history, change tickets | Firewall/cloud platform/ITSM | Export/API | Rules exist without business justification |
| Segmentation | Segmentation test output, routing evidence, trust-boundary diagrams | Test tooling/network platform | Test + export | Scope reduction assumed but never technically validated |
| Secure configuration | Baseline policy, configuration compliance report | Endpoint/cloud/config platform | Automated | Baseline documented but drift not monitored |
| Data retention | Retention configuration and deletion evidence | Application/storage platform | Configuration + logs | Sensitive data retained in logs/backups unexpectedly |
| Encryption | Encryption settings, key metadata, rotation records | KMS/HSM/application config | Export/API | Encryption enabled but key ownership unclear |
| Certificates/TLS | Certificate inventory, protocol configuration | Load balancer/API gateway/scanner | Automated | Deprecated protocol remains on overlooked endpoint |
| Endpoint protection | Coverage report, policy configuration, detections | EDR/endpoint platform | Automated | Assets missing agent/coverage |
| Vulnerability management | Scan results, remediation tickets, SLA report | VM platform/ITSM | Automated | Findings repeatedly reopen or ageing is hidden by rescans |
| Patch management | Deployment records, exception records | Patch/config platform | Automated | Patch deployment not linked to vulnerability risk |
| Application security | SAST/SCA/DAST results, code-review evidence | CI/CD and AppSec tools | Pipeline-generated | Security testing happens after release |
| Change control | Change record, approvals, deployment logs | ITSM/CI-CD | Automated + workflow | Emergency changes bypass review with no retrospective validation |
| User access | Authoritative user listing, role assignments | IdP/IAM/application | API/export | Local accounts outside central IAM |
| Privileged access | PAM records, admin group membership, session evidence | PAM/IdP | Automated | Standing admin access accumulates |
| Joiner/mover/leaver | Provisioning/deprovisioning events | HRIS/IAM/ITSM | Automated | Leavers removed from IdP but remain in downstream systems |
| MFA | Policy configuration, authentication methods, exclusions | IdP | Export/API | Exceptions and recovery paths undermine MFA |
| Service identities | Non-human identity inventory, owner, credentials | IAM/secrets platform | Automated | Orphaned service accounts and long-lived secrets |
| Physical security | Badge access, visitor process, media records | Facilities system | Export/review | Evidence retained inconsistently |
| Logging | Log-source inventory, ingestion/health status | SIEM/log platform | Automated | Expected logs configured but not actually arriving |
| Monitoring | Detection rules, alerts, triage records | SIEM/SOC platform | Automated | Rules exist but alerts are not actioned |
| Time synchronization | Configuration/state evidence | Infrastructure/config platform | Automated | Isolated systems drift |
| External scanning | Applicable scan reports and remediation | ASV/scanning platform | Scheduled | Failed findings accepted without proper closure |
| Penetration testing | Scope, methodology, report, retest | Testing provider/internal team | Periodic | Test scope does not reflect current architecture |
| Incident response | Plan, contact tree, exercise evidence, incident tickets | IR platform/docs | Periodic/event-driven | Plan exists but roles are not exercised |
| Security awareness | Completion data and role-based training | LMS | Automated | Generic training without role-specific content |
| Third parties | Responsibility matrix, contractual commitments, attestations | TPRM/procurement | Workflow | Reliance on vendor without understanding shared responsibility |
| Risk analysis | Targeted risk analysis record, assumptions, approval | GRC platform | Workflow | Analysis used to justify convenience rather than assess risk |
| Exceptions | Risk acceptance, expiry, safeguards | GRC/ITSM | Workflow | No expiry or accountable remediation owner |

## Evidence record template

Use the following structure for each evidence item:

```text
Evidence ID:
Control ID:
PCI mapping:
System/process:
Control owner:
Evidence owner:
Period covered:
Collection method:
Source system:
Artifact/location:
Expected result:
Observed result:
Exceptions/failures:
Related remediation ticket(s):
Reviewer:
Review date:
```

## Evidence quality test

Before accepting an artifact, ask:

1. Can an independent reviewer identify the system and population covered?
2. Does it prove operation during the relevant period?
3. Does it show the result, not merely the configuration intent?
4. Can failures be identified?
5. Can failures be traced to remediation or accepted risk?
6. Could the evidence be regenerated from the authoritative source?

If the answer to several of these is no, the evidence is probably documentation rather than assurance.

## Automating evidence collection

High-value automation candidates include:

- IAM group membership and MFA policy exports;
- cloud asset inventory;
- firewall and security-group configurations;
- EDR coverage;
- vulnerability status and SLA ageing;
- CI/CD security scan results;
- logging-source health;
- certificate inventories;
- encryption configuration;
- exception expiry alerts.

Automation should preserve context. A raw API response is not necessarily useful evidence unless the collection explains what population was queried and how the result maps to the control.

## Evidence retention model

For each evidence source, define:

| Field | Example |
| --- | --- |
| Evidence owner | IAM Operations |
| Frequency | Monthly |
| Source | Identity platform API |
| Storage | Restricted evidence repository |
| Naming convention | `YYYY-MM-control-system` |
| Reviewer | PCI control owner |
| Retention | Per assessment/legal/organizational policy |
| Failure escalation | Security governance ticket |

## Common anti-patterns

### Screenshot compliance

A screenshot can be useful corroboration, but it is weak as the primary evidence for a recurring control. Prefer exports, logs or API-generated reports.

### Evidence created immediately before assessment

For recurring controls, assessors and internal reviewers need to understand whether the control operated throughout the period. Build evidence collection into BAU.

### Policy equals implementation

A policy proves management intent. It does not prove the technical control exists or operates.

### Evidence without scope

A vulnerability report showing 100% remediation is meaningless if no one can establish whether the report contained every in-scope asset.

### Green dashboards without exceptions

A mature evidence model makes control failures visible. Perfect dashboards with no trace of operational problems should trigger scrutiny, not confidence.

## Suggested evidence maturity scale

| Level | Description |
| --- | --- |
| 1 — Manual | Evidence assembled manually for assessments |
| 2 — Repeatable | Defined owners, locations and collection cadence |
| 3 — System-generated | Evidence regularly exported from authoritative systems |
| 4 — Integrated | Evidence tied to controls, assets, exceptions and remediation |
| 5 — Continuous | Control state monitored automatically with failure alerts |

The objective is not to automate every artifact. It is to make important controls observable, repeatable and defensible.
