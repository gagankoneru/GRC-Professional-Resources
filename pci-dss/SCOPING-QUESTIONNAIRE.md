# PCI DSS Scoping Questionnaire

A practitioner questionnaire to support discovery of payment-data flows, connected systems, security-impacting systems, and third-party dependencies before formal assessment.

> This is an independent discovery aid, not an official PCI SSC scoping document.

## Payment flow

- Where does payment/account data first enter the environment?
- Which applications, APIs, queues, databases or services process it?
- Where can it be stored temporarily or permanently?
- Which logs, backups, analytics systems or support tools may receive copies?
- Where does the data leave the organization?
- Which parties receive it?

## Architecture

- Which network segments contain CDE assets?
- Which systems can initiate connections into those segments?
- Which systems receive connections from the CDE?
- Which administrative platforms can change CDE configurations?
- Which identity platforms authenticate CDE users or administrators?
- Which DNS, certificate, secrets, logging, endpoint, vulnerability or deployment platforms can materially affect CDE security?

## Cloud and platform services

- Which cloud accounts/subscriptions/projects host payment-related workloads?
- Are shared management planes used by both CDE and non-CDE workloads?
- Which IAM roles can administer those resources?
- Which CI/CD systems can deploy to the CDE?
- Which secret stores/KMS services provide credentials or keys?
- Which container registries, artifact stores or package sources feed CDE workloads?

## Third parties

For each service provider, identify:

- service provided;
- whether account data is stored, processed or transmitted;
- whether the provider can affect CDE security;
- connection method;
- responsibility split;
- relevant compliance/assurance evidence;
- incident-notification route;
- contract owner.

## Scope reduction and segmentation

- What technical boundary is being relied upon to exclude systems from scope?
- Is the boundary enforced by configuration or merely by process?
- Can credentials or administrative tooling cross the boundary?
- Has segmentation been independently tested?
- Can changes to cloud networking or routing silently invalidate the boundary?
- Is there monitoring for configuration drift?

## Data discovery

Search beyond expected databases. Consider:

- application logs;
- debug traces;
- support tickets;
- message queues;
- data lakes;
- analytics platforms;
- backups/snapshots;
- object storage;
- developer/test environments;
- exported reports;
- local administrator workstations.

## Scoping output

The scoping process should produce at least:

```text
CDE system inventory:
Connected/security-impacting system inventory:
Payment-data flow diagram:
Network/trust-boundary diagram:
Third-party inventory:
Identity/administration dependencies:
Scope exclusions and technical rationale:
Segmentation evidence:
Outstanding discovery questions:
Scope owner:
Review date:
```

## Trigger events for scope reassessment

Revisit scope when there is:

- a new payment channel;
- major architecture or cloud migration;
- a new third party;
- a change to authentication/administration tooling;
- new connectivity into the CDE;
- acquisition or organizational integration;
- new logging/analytics destination;
- material CI/CD change;
- segmentation redesign;
- discovery of previously unknown account-data storage.

PCI scoping should be treated as a living architecture record rather than an annual questionnaire.
