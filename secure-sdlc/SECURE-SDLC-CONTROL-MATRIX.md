# Secure SDLC Control Matrix

A practical control matrix for integrating security into software delivery without turning the SDLC into a compliance-only process.

## Operating principle

Security controls should appear as early as practical, produce evidence automatically where possible, and become stricter as the potential impact of a system increases.

A useful lifecycle is:

**Plan → Design → Build → Test → Release → Operate → Learn**

## Risk tiers

Before applying gates, classify the application or material change.

| Tier | Example profile | Expected security depth |
| --- | --- | --- |
| Tier 1 — Critical | Payment, identity, privileged administration, regulated/high-impact services | Full control set, formal threat model, security approval for material changes |
| Tier 2 — High | Sensitive-data or externally exposed business service | Strong automated testing and risk-based manual review |
| Tier 3 — Standard | Internal or lower-impact application | Baseline automated controls and engineering ownership |
| Tier 4 — Low | Prototype/non-sensitive utility with no production trust | Lightweight controls; no exemption from secrets/dependency hygiene |

Classification should consider data, privilege, exposure, business impact, regulatory scope, architecture and downstream dependencies.

## Control matrix

| Phase | Control objective | Example control | Evidence | Gate / trigger |
| --- | --- | --- | --- | --- |
| Plan | Identify security obligations | Security/privacy/regulatory requirements captured in backlog | Requirements or acceptance criteria | Before design for Tier 1/2 |
| Plan | Establish ownership | Named application and security-risk owner | Service catalogue/CMDB | Before production onboarding |
| Plan | Classify risk | Application/change risk tier assigned | Risk record | Before selecting required controls |
| Design | Understand attack surface | Architecture and trust boundaries documented | Diagram/design record | Material architecture change |
| Design | Identify threats | Threat model or structured abuse-case review | Threat-model record | Required for Tier 1; risk-based for Tier 2 |
| Design | Reduce privilege | Identities, roles and authorization model reviewed | IAM design | New auth/authz path |
| Design | Protect sensitive data | Data flows, classification, retention and crypto design reviewed | Data-flow/design record | Sensitive/regulated data |
| Design | Assess dependencies | External services and critical libraries considered | Design/vendor record | New material dependency |
| Build | Prevent secrets exposure | Secret scanning and approved secret storage | Pipeline result/configuration | Every commit/build where supported |
| Build | Detect insecure code patterns | Static analysis | SAST result | Pull request/build |
| Build | Control dependencies | Software composition/dependency analysis | SCA/SBOM result | Build + recurring rescans |
| Build | Maintain code quality | Peer review / protected branch workflow | PR review history | Merge |
| Build | Protect CI/CD | Least privilege, protected credentials, controlled runners | Platform configuration | Continuous |
| Test | Validate exposed behavior | Dynamic application/API testing | DAST/API test output | Pre-release based on risk |
| Test | Test authorization | Positive and negative access-control tests | Automated/manual test evidence | Tier 1/2 and auth changes |
| Test | Validate threat mitigations | Security tests mapped to material threats | Test cases/results | Threat-modelled risks |
| Test | Find exploitable weaknesses | Penetration testing / focused manual testing | Report and retest | Risk/event driven |
| Release | Prevent unacceptable risk | Vulnerability/security gate | Pipeline decision | Every production release |
| Release | Ensure traceability | Release linked to commit/build/artifacts | CI/CD provenance | Every release |
| Release | Govern exceptions | Time-bound security exception | Approved risk record | Gate failure overridden |
| Release | Protect artifacts | Integrity/signing/provenance controls where appropriate | Build/signing metadata | High assurance workloads |
| Operate | Detect new risk | Recurring dependency/container/host scanning | Scan output | Continuous/scheduled |
| Operate | Observe attacks/failures | Security logging and detection coverage | SIEM/telemetry | Production |
| Operate | Fix vulnerabilities | Risk-based SLA and remediation ownership | Tickets/metrics | Finding created |
| Operate | Protect identity | Access review and credential lifecycle | IAM/PAM evidence | Scheduled/event-driven |
| Learn | Improve from incidents | Root cause feeds engineering controls | PIR/control change | Security incident |
| Learn | Tune security controls | False-positive/escape analysis | AppSec metrics | Periodic |
| Learn | Reassess threat model | Architecture/threat model updated | Revised model | Material change |

## Minimum control baseline

Every production software project should normally have:

- [ ] Named service/application owner
- [ ] Repository access control
- [ ] Peer-reviewed changes
- [ ] Protected production deployment path
- [ ] Secrets scanning
- [ ] Dependency/SCA scanning
- [ ] Vulnerability remediation process
- [ ] Centralized secret management
- [ ] Security-relevant logging
- [ ] Defined incident ownership
- [ ] Documented exception process

Higher-risk systems should add threat modeling, stronger testing, authorization validation, penetration testing, software provenance and formal release-security gates.

## Security requirements as engineering acceptance criteria

Avoid requirements such as:

> The application must be secure.

Prefer testable outcomes such as:

> A standard user cannot invoke administrative API operations, including by directly calling the endpoint rather than using the UI.

or:

> Application secrets are retrieved from the approved secret store at runtime and are not present in source, build logs or deployable artifacts.

A good security requirement has:

1. a threat or failure condition;
2. an expected control behavior;
3. a test method;
4. an observable pass/fail result.

## Threat modeling prompts

For material changes, ask:

### Identity

- Who or what can authenticate?
- Can identity be spoofed?
- Are service/workload identities distinguishable from users?
- Can privilege be inherited or delegated unexpectedly?

### Authorization

- What high-impact actions exist?
- Where is authorization enforced?
- Can clients bypass UI-layer restrictions?
- Are tenant/resource boundaries enforced server-side?

### Data

- What sensitive data enters or leaves the system?
- Could logs, caches, queues or backups create unintended copies?
- What is the retention requirement?
- Can lower-trust components influence higher-trust data flows?

### Inputs and execution

- Which inputs can alter queries, commands, templates or interpreters?
- Does the application invoke operating-system, database or cloud APIs?
- What happens with malformed, oversized or adversarial input?

### Dependencies

- Which third-party packages/services are security-critical?
- How are updates authenticated and reviewed?
- Can a compromised dependency reach secrets or production systems?

### Failure

- What happens when an upstream service is unavailable or malicious?
- Does the system fail open?
- Are retry and fallback paths equally protected?

## Vulnerability release-gate example

A simple policy can begin with:

| Finding | Default decision |
| --- | --- |
| Known exploitable critical vulnerability in release path | Block |
| Critical vulnerability with credible exploitability | Block |
| High vulnerability affecting exposed/high-value component | Block or explicit risk approval |
| Medium/low | Track against defined remediation SLA |
| False positive | Document suppression with reason and reviewer |

Severity alone should not make the decision. Consider reachability, exploitability, exposure, privilege, compensating controls and asset criticality.

## Exception model

A security exception must not become a permanent bypass.

Record:

```text
Exception ID:
Application/service:
Failed control/gate:
Finding/risk:
Business reason:
Exposure and exploitability:
Interim safeguards:
Accountable owner:
Security reviewer:
Approval:
Expiry date:
Target remediation date:
Linked engineering work:
```

An expired exception should return the control to a failed state.

## Engineering metrics

Useful SDLC metrics include:

- percentage of repositories with required security scanning enabled;
- percentage of critical applications with current threat models;
- mean/median vulnerability age by risk tier;
- vulnerabilities reopened or recurring after remediation;
- percentage of releases blocked by security controls;
- exception volume and age;
- secrets detected before versus after merge;
- dependency exposure window;
- security defects originating in design versus implementation;
- percentage of material threats with validating security tests.

Avoid using raw vulnerability counts to compare engineering teams without accounting for application size, exposure and testing depth.

## Mapping to assurance frameworks

The same Secure SDLC evidence can support multiple assurance obligations. Instead of building separate SDLC processes for PCI DSS, ISO 27001, SOC 2 or internal policy, maintain one engineering control set and map those controls to applicable frameworks.

The internal control should remain stable even when framework language changes.

## Definition of done for a security control

A control is not complete because a policy mentions it. For an engineering-facing SDLC control, aim to establish:

**owner + implementation + scope + automated/manual test + evidence + failure path + remediation**

That structure makes the control useful both to developers and to assurance teams.
