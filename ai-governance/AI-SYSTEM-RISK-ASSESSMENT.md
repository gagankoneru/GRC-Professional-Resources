# AI System Risk Assessment

A practical, engineering-aware method for assessing the security, governance, privacy, and operational risks of AI systems before deployment and throughout their lifecycle.

## Why this exists

Traditional technology risk assessments often treat an application as a relatively static combination of software, infrastructure, data, and users. AI systems introduce additional uncertainty because behavior may depend on model capability, prompts, retrieved context, training or fine-tuning data, connected tools, autonomous actions, and changing model versions.

This assessment therefore evaluates both the **system around the model** and the **behavior enabled by the model**.

---

## 1. System Identification

Record enough information for another security or engineering reviewer to understand what is actually being assessed.

| Field | Description |
|---|---|
| System / product name | Name of the AI-enabled service or capability |
| Business owner | Accountable business owner |
| Technical owner | Engineering or platform owner |
| Security owner | Security contact responsible for assurance |
| Model(s) | Model provider, model family, and version where known |
| Deployment model | SaaS, API, self-hosted, embedded, edge, hybrid |
| AI pattern | Assistant, classifier, RAG, agent, copilot, decision support, autonomous workflow, other |
| Intended users | Employees, customers, partners, public, machines/agents |
| Environments | Development, test, production |
| Review date | Date of the assessment |
| Next review trigger | Date or event that requires reassessment |

---

## 2. Intended Purpose and Decision Authority

### Intended purpose

Describe the business problem the system is intended to solve and the decisions or actions it supports.

### Prohibited use

Document explicitly what the AI system must **not** be used for.

### Decision authority

Classify the system using the highest applicable level:

| Level | Description | Example |
|---|---|---|
| A0 | Informational only | Summarises documents |
| A1 | Recommends | Suggests a remediation action to an analyst |
| A2 | Acts with approval | Prepares an action that a human must approve |
| A3 | Acts within bounded authority | Performs pre-approved low-risk actions autonomously |
| A4 | High-impact autonomous action | Can materially affect users, systems, money, access, or regulated outcomes without prior human approval |

**Governance principle:** increasing decision authority should require stronger identity, authorization, logging, testing, rollback, and human-oversight controls.

---

## 3. Architecture and Trust Boundaries

Document the major components and data flows:

```text
User / Calling System
        |
        v
Application / Agent
        |
        +----> Model Provider
        |
        +----> Retrieval / Knowledge Sources
        |
        +----> Tools / APIs / Plugins
        |
        +----> Memory / State
        |
        +----> Logging / Monitoring
```

For each boundary, identify:

- identity used;
- authentication method;
- authorization decision;
- data transmitted;
- data retained;
- encryption expectations;
- external party involved;
- failure mode if the component is compromised.

---

## 4. Data Assessment

### Data categories

Mark all that apply:

- [ ] Public data
- [ ] Internal business data
- [ ] Confidential data
- [ ] Personal data
- [ ] Special-category / sensitive personal data
- [ ] Authentication or authorization data
- [ ] Credentials, secrets, or tokens
- [ ] Source code
- [ ] Security-sensitive configuration
- [ ] Financial information
- [ ] Intellectual property
- [ ] Regulated records
- [ ] Customer-provided content

### Questions

1. What information can enter model context?
2. Can prompts or retrieved documents contain secrets?
3. Can model outputs expose information from another user, tenant, session, or source?
4. Is submitted data retained or used by a third-party provider for model improvement?
5. Are deletion and retention requirements technically enforceable?
6. Is data residency relevant?
7. Can generated content become a new system of record?
8. Can untrusted external content enter retrieval, memory, or agent context?

---

## 5. Model and Supply-Chain Risk

Assess:

- model provider due diligence;
- model provenance;
- model/version change management;
- dependency and package integrity;
- third-party AI SDKs;
- extensions, plugins, tools, or MCP servers;
- externally sourced models or adapters;
- fine-tuning data provenance;
- vulnerability and incident notification commitments;
- provider business continuity and exit strategy.

### Key question

**What can change outside your deployment pipeline while still changing system behavior?**

Examples include a provider updating a hosted model, an external knowledge source changing, a tool schema being modified, or an upstream prompt/template dependency changing.

---

## 6. AI Security Threat Assessment

Rate each scenario for **Likelihood (1-5)** and **Impact (1-5)** before and after controls.

| Risk | Example scenario | Key control themes |
|---|---|---|
| Prompt injection | Untrusted content alters intended model behavior | input trust boundaries, context separation, least privilege, output validation |
| Sensitive information disclosure | Model reveals secrets or restricted data | data minimisation, access filtering, redaction, tenant isolation |
| Excessive agency | Agent performs an action beyond intended authority | tool allowlists, scoped credentials, human approval, transaction limits |
| Tool misuse | Valid tool is invoked with unsafe parameters | authorization, parameter validation, execution policy |
| Identity / privilege abuse | Agent inherits or escalates excessive privileges | workload identity, delegated authorization, short-lived credentials |
| Retrieval poisoning | Malicious content enters a knowledge base or RAG source | source provenance, ingestion controls, content review, isolation |
| Memory poisoning | Malicious or erroneous state persists across interactions | memory scope, write controls, provenance, reset and review capabilities |
| Insecure output handling | Model output is interpreted as executable or trusted content | encoding, validation, sandboxing, separation of data and instructions |
| Unexpected code execution | Generated or supplied content reaches an execution environment | sandboxing, command allowlists, isolation, approval gates |
| Model / dependency compromise | Upstream component introduces malicious behavior | provenance, signing, version pinning, supplier assurance |
| Denial / resource exhaustion | Token, tool, or compute use creates material disruption or cost | quotas, rate limits, timeouts, budgets, circuit breakers |
| Hallucination / integrity failure | Incorrect output is treated as authoritative | grounding, verification, confidence handling, human review |
| Monitoring blind spot | Harmful behavior occurs without sufficient forensic evidence | structured logs, decision traces, tool-call logging, retention |

---

## 7. Agentic AI Control Review

Complete this section whenever the system can invoke tools or take actions.

### Identity

- Does the agent have its own identifiable workload identity?
- Is user identity preserved when authority is delegated?
- Can downstream systems distinguish the user, agent, and service identities?

### Authorization

- Are tools explicitly allowlisted?
- Are permissions scoped to the minimum actions required?
- Can the agent modify its own permissions or toolset?
- Is authorization checked at the tool boundary rather than only in the prompt?

### Action controls

- Which actions require human approval?
- Are financial, destructive, external-communication, access-control, or data-disclosure actions subject to stronger gates?
- Are transaction/value/rate limits enforced technically?
- Can an unsafe action be rolled back?

### Execution integrity

- Are tool parameters validated independently of model output?
- Can the model bypass the expected execution gateway?
- Is untrusted model-generated content ever executed directly?

### Observability

Can investigators reconstruct:

1. who initiated the interaction;
2. which model/version was used;
3. what data sources influenced the decision;
4. which tools were offered;
5. which tool was selected;
6. parameters submitted;
7. authorization/approval decisions;
8. resulting system changes?

---

## 8. Human Oversight

Define when human involvement is required.

| Trigger | Required response |
|---|---|
| Low confidence / ambiguous result | Escalate or require confirmation |
| High-impact action | Human approval before execution |
| Policy conflict | Fail closed and escalate |
| Unexpected tool selection | Block or quarantine |
| Sensitive-data detection | Restrict output / invoke handling procedure |
| Repeated anomalous behavior | Disable capability and investigate |

Avoid using "human in the loop" as a control unless the human receives enough context, time, authority, and information to make a meaningful decision.

---

## 9. Testing and Validation

Testing should reflect the actual deployed architecture, not only the base model.

Minimum test areas:

- [ ] expected use cases
- [ ] prohibited use cases
- [ ] prompt injection
- [ ] indirect prompt injection
- [ ] sensitive-data leakage
- [ ] cross-user / cross-tenant isolation
- [ ] unsafe tool invocation
- [ ] privilege boundaries
- [ ] retrieval poisoning
- [ ] malformed model output
- [ ] excessive resource consumption
- [ ] fallback behavior
- [ ] logging and incident reconstruction
- [ ] model/provider version change
- [ ] rollback / kill switch

Record test cases as repeatable artifacts where possible so regressions can be detected when models, prompts, tools, or data sources change.

---

## 10. Risk Scoring

### Inherent risk

`Inherent Risk = Likelihood x Impact`

| Score | Rating |
|---|---|
| 1-4 | Low |
| 5-9 | Moderate |
| 10-16 | High |
| 17-25 | Critical |

### Residual risk

Score the same scenario again after considering implemented controls.

A control should reduce likelihood or impact only when there is evidence that it is **implemented and operating**, not merely documented.

### Recommended decision model

| Residual risk | Typical decision |
|---|---|
| Low | Approve with normal monitoring |
| Moderate | Approve with tracked actions / conditions |
| High | Senior risk acceptance or remediation before deployment |
| Critical | Do not deploy until reduced, except through an explicitly governed exceptional process |

Organizations should adapt thresholds to their own risk appetite rather than treating these values as universal.

---

## 11. Evidence

Attach or link evidence supporting control claims:

- architecture diagram;
- data-flow diagram;
- threat model;
- supplier assessment;
- privacy assessment;
- access-control configuration;
- tool permission definitions;
- test results;
- red-team results;
- model/system card;
- logging samples;
- incident and rollback procedure;
- approval records;
- outstanding remediation items.

---

## 12. Deployment Decision

### Decision

- [ ] Approved
- [ ] Approved with conditions
- [ ] Reassessment required
- [ ] Not approved

### Conditions / risk treatment

Document each action with:

| Action | Owner | Due date | Risk addressed | Evidence required |
|---|---|---|---|---|
| | | | | |

### Risk acceptance

Where residual risk exceeds normal tolerance, document:

- specific risk accepted;
- business justification;
- accountable approver;
- expiry/review date;
- compensating controls;
- conditions that invalidate the acceptance.

---

## 13. Continuous Review Triggers

Reassess when any of the following materially changes:

- model or model version;
- system prompt or orchestration logic;
- connected tools or permissions;
- retrieval sources;
- memory architecture;
- data classification;
- user population;
- intended purpose;
- autonomy level;
- hosting architecture;
- third-party provider;
- relevant threat intelligence;
- material security incident;
- applicable legal or regulatory requirements.

---

## Core principle

> Governance should constrain **capability and authority**, not merely document that an AI system exists.

The strongest AI governance controls are those that engineering teams can implement, test, observe, and prove are operating.