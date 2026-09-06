# Secure SDLC Security Gates Checklist

A concise, reusable checklist for deciding whether a software change is ready to move from design to build, build to release, and release to production.

The checklist is intentionally risk-based. Not every change needs the same depth of review, but every production change should have a defensible security path.

## Gate 0 — Risk classification

Complete before selecting the security controls for the change.

- [ ] Application/service owner identified
- [ ] Change owner identified
- [ ] Production exposure understood
- [ ] Data classification understood
- [ ] Authentication/authorization impact assessed
- [ ] Privileged functionality identified
- [ ] Regulatory or contractual scope identified
- [ ] Internet exposure identified
- [ ] Third-party dependencies identified
- [ ] Application/change risk tier assigned

### Escalate when

- payment or regulated data is introduced;
- authentication or authorization changes;
- a new internet-facing surface is created;
- a component gains privileged cloud/system access;
- a new critical third party or dependency is introduced.

---

## Gate 1 — Design security

Complete before implementation for material/high-risk changes.

### Architecture

- [ ] Architecture diagram reflects the proposed change
- [ ] Trust boundaries identified
- [ ] External interfaces identified
- [ ] High-impact actions identified
- [ ] Administrative paths identified

### Identity and access

- [ ] Authentication mechanism defined
- [ ] Authorization is enforced server-side
- [ ] Least-privilege roles defined
- [ ] Service/workload identities have owners
- [ ] Privileged access is separated from normal user access
- [ ] Recovery/bypass paths are considered

### Data protection

- [ ] Sensitive data flows documented
- [ ] Data minimization considered
- [ ] Encryption needs identified
- [ ] Key/secret ownership defined
- [ ] Retention and deletion requirements defined
- [ ] Logging avoids unnecessary sensitive data

### Threat assessment

- [ ] Material abuse cases identified
- [ ] Threat model completed when required by risk tier
- [ ] Mitigations assigned to engineering work
- [ ] Security assumptions recorded
- [ ] Residual high risks have accountable owners

**Gate fails if:** the team cannot explain who can perform high-impact actions, where authorization is enforced, or where sensitive data moves.

---

## Gate 2 — Code and build security

Complete before merge/release candidate creation.

- [ ] Peer review completed
- [ ] Required branch protections satisfied
- [ ] Secrets scanning passed
- [ ] Static analysis completed where applicable
- [ ] Dependency/SCA scan completed
- [ ] Critical/high findings triaged
- [ ] Build dependencies originate from approved/trusted sources
- [ ] CI/CD credentials are not embedded in code or build definitions
- [ ] Build runner permissions follow least privilege
- [ ] Security-sensitive configuration is separated from source where appropriate

### Supply-chain questions

- [ ] New dependencies have a clear purpose
- [ ] Dependency ownership/maintenance status is acceptable
- [ ] Known vulnerabilities have been assessed
- [ ] Lockfiles or equivalent version controls are used where appropriate
- [ ] Build outputs are traceable to source revision

**Gate fails by default if:** a credential is committed, an unassessed critical vulnerability is reachable in the release path, or the build cannot be traced to reviewed source.

---

## Gate 3 — Security validation

Complete before production release according to risk tier.

- [ ] Security acceptance criteria tested
- [ ] Authentication tested
- [ ] Authorization negative tests completed for high-risk functionality
- [ ] Input-validation/security abuse cases tested
- [ ] Security-relevant error handling tested
- [ ] DAST/API testing completed where applicable
- [ ] Threat-model mitigations have corresponding validation
- [ ] Penetration/focused manual testing completed when required
- [ ] Findings linked to remediation work
- [ ] Retests completed for release-blocking findings

### Negative testing examples

Attempt to:

- access another user's/tenant's resource;
- call privileged endpoints directly;
- modify object identifiers;
- replay or reuse authorization artifacts;
- submit malformed or oversized inputs;
- bypass workflow sequencing;
- access deleted/disabled identities;
- trigger sensitive actions without expected approval.

**Gate fails if:** a release-blocking vulnerability remains unresolved without an explicit, time-bound risk decision.

---

## Gate 4 — Production readiness

- [ ] Production secrets come from approved secret management
- [ ] Production access is restricted
- [ ] Logging is enabled for security-relevant events
- [ ] Detection/alert routing is defined for critical events
- [ ] Vulnerability monitoring is enabled
- [ ] Backup/recovery requirements are satisfied where applicable
- [ ] Rollback path is defined
- [ ] Incident owner/on-call route is known
- [ ] Asset/service inventory is updated
- [ ] Security exceptions are documented and unexpired

### Release traceability

Record:

```text
Release/version:
Source commit:
Build ID:
Change/PR:
Security test results:
Known accepted risks:
Approver(s):
Deployment timestamp:
```

---

## Gate 5 — Post-release assurance

After release:

- [ ] Confirm expected telemetry is arriving
- [ ] Confirm critical security controls operate in production
- [ ] Review unexpected authorization/error events
- [ ] Confirm new assets are included in vulnerability monitoring
- [ ] Validate that temporary deployment privileges were removed
- [ ] Close or track remaining remediation items
- [ ] Update threat model/documentation if implementation differed from design

For high-impact changes, perform a short security verification after deployment rather than assuming staging behavior equals production behavior.

---

## Emergency changes

Emergency deployment should change the timing of security assurance, not eliminate it.

Minimum expectations:

- [ ] Emergency rationale recorded
- [ ] Named accountable approver
- [ ] Minimum automated security controls still executed where technically possible
- [ ] Scope of bypass documented
- [ ] Post-deployment security review scheduled immediately
- [ ] Retrospective review completed
- [ ] Any temporary access/configuration removed

Repeated use of the emergency path should be treated as a governance signal that the normal delivery process needs improvement.

---

## Security exception decision

An exception should answer:

1. What control or gate failed?
2. What can realistically happen because of the failure?
3. What is the exposure and exploitability?
4. What interim safeguard reduces risk?
5. Who accepts the residual risk?
6. When does the exception expire?
7. What engineering work permanently resolves it?

An exception without an expiry date is not an exception; it is an undocumented change to the security baseline.

---

## Suggested automated release policy

Organizations can translate the checklist into pipeline policy. A simple starting model:

**Block automatically**

- exposed secret/credential;
- known exploitable critical vulnerability affecting the release;
- failed required security test;
- unapproved production deployment path.

**Require review**

- high-severity reachable vulnerability;
- new privileged functionality;
- material authentication/authorization change;
- new handling of regulated/sensitive data;
- expired security exception.

**Track to SLA**

- lower-risk findings that do not exceed the organization's release threshold.

The exact thresholds should reflect system risk rather than becoming universal severity rules.
