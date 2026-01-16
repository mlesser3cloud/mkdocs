---
# Hell's Pass Hospital — Azure BCDR Failover Plan (24x7)

**Document status:** Draft for implementation  
**Audience:** Clinical Systems, Infrastructure, Security, Application Owners, Service Desk, Executive Leadership  
**Scope:** Azure-hosted applications and supporting services required for hospital operations, including EHR-related workflows and integrations.

> This document is written to be placed directly into MkDocs as a single Markdown page.

---

<!-- PAGE 1 -->

## 1. Document control

### 1.1 Purpose
This plan defines how Hell’s Pass Hospital will maintain 24x7 operations through outages and disasters using high availability (HA), disaster recovery (DR), and backups.

### 1.2 Definitions
- **BC (Business Continuity):** Keeping critical services running (or quickly restored) through disruption.
- **HA (High Availability):** Design to avoid downtime during common failures (host, rack, zone, instance).
- **DR (Disaster Recovery):** Ability to restore service after major incidents (regional outage, data destruction, ransomware).
- **RTO (Recovery Time Objective):** Maximum acceptable downtime.
- **RPO (Recovery Point Objective):** Maximum acceptable data loss.

### 1.3 Assumptions
- Core clinical workflows must remain available 24x7, including during nights/weekends.
- “Failover” decisions must be executable by on-call staff with clear authority.
- DR is not complete without routine testing and measured recovery times.

### 1.4 Out of scope (examples)
- On-premises-only systems not connected to Azure.
- Vendor-managed environments where hospital lacks administrative control (unless specifically integrated).

---

<!-- PAGE 2 -->

## 2. Executive intent and objectives

### 2.1 Objectives
- Maintain continuous access to patient care systems (clinical Tier 0/Tier 1).
- Minimize downtime and data loss for all covered systems using tiered RTO/RPO targets.
- Provide repeatable failover runbooks with clear decision points and validation steps.
- Ensure recoverability from non-outage events (corruption, accidental deletion, ransomware).

### 2.2 Guiding principles
- **Prefer HA over DR** for common failure modes to avoid invoking regional failover.
- **Treat backups as last-resort recovery** for destructive/corruptive events (since replication can replicate bad data).
- **Automate and standardize** provisioning and failover steps to reduce human error.
- **Test what is written**: plans must be exercised and refined after each drill/incident.

### 2.3 Business impact statement (hospital context)
- Patient safety risk increases rapidly when medication administration, orders, allergies, and clinical documentation are unavailable.
- Downtime can force manual workflows that increase clinical burden and error.
- Loss of integration (HL7/FHIR interfaces) can create silent failures where systems appear “up” but data stops flowing.

---

<!-- PAGE 3 -->

## 3. Tiering model and recovery targets

### 3.1 System tiers
Define tiers for all applications and dependencies (examples below—replace with your inventory):

| Tier | Category | Examples | Target RTO | Target RPO |
|---|---|---|---:|---:|
| Tier 0 | Life/safety | EHR core clinical, med admin, identity/auth, interface engine | 15–30 min | 0–5 min |
| Tier 1 | Mission essential | PACS/RIS access, lab result viewing, ED tracking boards | 1–4 hrs | 15–60 min |
| Tier 2 | Business critical | Revenue cycle, HR, internal reporting | 8–24 hrs | 4–24 hrs |
| Tier 3 | Non-critical | Dev/test, low priority apps | 24–72 hrs | 24–72 hrs |

### 3.2 Service dependency mapping (required)
For each Tier 0–2 system, document:
- Upstream/downstream integrations (APIs, HL7 feeds, event buses)
- Identity dependencies (SSO, MFA, service principals)
- Data stores and replication mode
- Network dependencies (private endpoints, VPN/ExpressRoute, DNS, firewalls)
- Third-party vendor dependencies

**Deliverable:** `System Dependency Map` diagram + a table of “hard dependencies” per system.

---

<!-- PAGE 4 -->

## 4. Azure resilience architecture (target state)

### 4.1 Region and zone strategy
- Primary production workloads run in a designated **Primary Region** with multi-instance design and (where supported) multiple Availability Zones for HA.
- DR capability is implemented in a **Secondary Region**, using a pre-built “warm standby” or “active/active” design depending on tier and cost constraints.
- Region selection should consider Microsoft guidance on paired regions[^1] for broad outage recovery behaviors.

**Fill in:**
- Primary Region: `__________`
- Secondary Region: `__________`
- DNS strategy: `__________`
- Traffic failover method: `__________` (e.g., global load balancing)

### 4.2 Workload patterns (choose per system)
- **Active/Active (Tier 0 where feasible):** Both regions serve traffic; failure routes to remaining region.
- **Warm standby (common Tier 0–1):** Secondary is running at reduced capacity; scale up on failover.
- **Cold standby (Tier 2–3):** Secondary is mostly infrastructure-as-code + backups; restore/rebuild during event.

### 4.3 Baseline Azure components (minimum)
- Hub/spoke network with segmentation by trust boundary (clinical vs corporate vs vendor).
- Central logging and monitoring.
- Key management and secret storage.
- Standardized compute patterns (VMSS, AKS, PaaS where possible).
- Immutable infrastructure deployment pipelines for repeatability.

---

<!-- PAGE 5 -->

## 5. Roles, authority, and communications

### 5.1 Incident roles (BCDR)
- **Incident Commander (IC):** Owns severity, decision to fail over, executive updates.
- **Clinical Liaison:** Communicates operational impacts and validates clinical workflow restoration.
- **Infrastructure Lead:** Executes platform failover steps and capacity changes.
- **App Owner(s):** Validates application-level health and business functions.
- **Security Lead:** Guides containment and forensics in ransomware/cyber events.
- **Service Desk Lead:** Manages user communications and incident ticketing.

### 5.2 Decision authority (pre-approved)
Define who can authorize:
- Regional failover (Primary → Secondary)
- Failback (Secondary → Primary)
- Ransomware containment actions (shutdown/isolation)
- Public communications to staff and partners

**Fill in names and backups (24x7):**
- IC Primary / IC Backup: `__________` / `__________`
- Infra Primary / Backup: `__________` / `__________`
- Clinical Primary / Backup: `__________` / `__________`
- Security Primary / Backup: `__________` / `__________`

### 5.3 Communications templates (minimum)
- “Service disruption acknowledged” (internal)
- “Failover in progress” (clinical leadership)
- “Read-only / downtime procedures” notice (if applicable)
- “Service restored / validation required” notice

---

<!-- PAGE 6 -->

## 6. Data protection and integrity design

### 6.1 Principles
- Use HA/replication for uptime and low RPO, but maintain independent backups for integrity recovery.
- Separate “availability recovery” (failover) from “integrity recovery” (restore to known-good state).
- Encrypt data in transit and at rest; protect keys and secrets from accidental exposure.

### 6.2 Data categories
Classify data to drive controls:
- **Clinical data** (PHI/PII, orders, results)
- **Imaging data** (large objects, long retention)
- **Operational logs** (audit/security)
- **Configuration and IaC** (code, templates, manifests)
- **Identity configuration** (critical for access restoration)

### 6.3 Replication vs backup (how to choose)
- Use replication/failover for **availability** objectives (fast restore of service).
- Use backups for **ransomware/corruption** and long-term retention requirements.

---

<!-- PAGE 7 -->

## 7. DR scenarios and response playbooks (overview)

### 7.1 Scenario catalog

| Scenario | Example symptoms | Primary response | Data recovery method |
|---|---|---|---|
| App instance failure | One node down | HA (scale set/replicas) | None |
| Availability Zone outage | Zonal services degrade | In-region failover to remaining zone(s) | None |
| Primary region outage | Region-wide failure | Regional failover to Secondary | Replication / DR orchestration |
| Network isolation | ExpressRoute/VPN down | Alternate connectivity + traffic failover | None |
| Ransomware | Encryption, IOC alerts | Contain + restore | Backups to clean environment |
| Data corruption | Bad writes, schema drift | Stop writes + restore point | Backup restore or point-in-time |
| Bad deployment | New release failure | Roll back | No data restore unless needed |

### 7.2 Failover decision tree (simplified)
1. Confirm severity and scope (single component vs zone vs region vs cyber).
2. If **cyber/ransomware suspected**, prioritize containment before failover.
3. If **regional outage** and Tier 0 impacted beyond tolerance, execute Regional Failover Runbook.
4. Validate clinical workflows end-to-end before declaring “restored.”

---

<!-- PAGE 8 -->

## 8. Failover runbook — Primary region outage (Tier 0/1)

### 8.1 Preconditions
- Secondary region environment exists (warm standby or active/active).
- Access paths exist for on-call staff (break-glass accounts, MFA, privileged workflows).
- Monitoring confirms outage exceeds RTO tolerance.

### 8.2 Execution steps (high level)
1. **Declare incident** and assign Incident Commander and functional leads.
2. **Freeze changes**: stop deployments and non-essential configuration changes.
3. **Initiate traffic failover** to the Secondary Region (global load balancing / DNS plan).
4. **Fail over data services** according to each system’s method (database failover group / replication mechanism).
5. **Scale Secondary Region** to meet expected patient-care load.
6. **Validate** critical workflows:
   - Login + MFA
   - Patient chart open
   - Orders entry
   - Medication administration record access
   - Lab result arrival
   - Interface engine message flow
7. **Communicate** “Restored on DR” status and known limitations.

### 8.3 Orchestration note (VM-centric stacks)
Azure Site Recovery can orchestrate VM failover with recovery plans that coordinate sequencing and grouped recovery.

### 8.4 Commit, stabilization, and operating in DR
- Declare “DR steady state” once monitoring is stable and clinical validation is complete.
- Increase logging and change control rigor while operating in DR.
- Begin planning for failback only after primary region stability is confirmed.

---

<!-- PAGE 9 -->

## 9. Failover runbook — Ransomware / destructive cyber event

### 9.1 Objectives
- Stop spread, preserve evidence, and restore clean operations safely.
- Avoid “failing over infected workloads” into the Secondary Region.

### 9.2 Immediate actions (first 15–30 minutes)
1. **Declare security incident** and engage Security Lead.
2. **Containment**:
   - Isolate affected subnets/workloads.
   - Disable compromised credentials and rotate secrets.
3. **Preserve evidence** per policy (logs, snapshots where appropriate).
4. **Assess blast radius** (which tiers/systems/data stores are impacted).

### 9.3 Recovery approach
- Stand up a clean recovery environment (secondary region or isolated subscription) using Infrastructure-as-Code.
- Restore data from known-good backups and validate integrity before reintroducing integrations.
- Re-enable traffic only after:
  - Identity is validated
  - Malware scanning/EDR checks pass
  - Clinical workflows pass acceptance tests

### 9.4 Validation checklist (minimum)
- No anomalous privileged sign-ins
- Interfaces sending/receiving expected volumes
- Database integrity checks pass
- Clinical spot-checks performed by nursing/clinical super-users

---

<!-- PAGE 10 -->

## 10. Backup strategy by technology (standards)

### 10.1 Backup policy standards (hospital)
- **Tier 0:** Frequent backups and short RPO aligned with clinical safety needs; include rapid restore drills.
- **Tier 1:** Regular backups with periodic restore validation.
- **Tier 2–3:** Longer retention, reduced frequency acceptable, but restore process must be documented.

### 10.2 Technology-specific strategy

| Technology | What to back up | Frequency (example) | Retention (example) | Notes |
|---|---|---:|---:|---|
| Azure VMs | VM backup (system + data disks) | Daily + critical pre-change | 30–90 days | Use app-consistent where possible. |
| SQL Server on Azure VM | Full/diff/log backups | 15 min logs (Tier 0), daily full | 30–180 days | Document restore order and validation. |
| Azure SQL Database | PITR + geo strategy | Per service defaults + policy | 7–35 days (typical) | Use failover grouping for DR where needed. |
| AKS | Cluster state + PV snapshots | Daily + pre-change | 30–90 days | Ensure restore runbook tested. |
| Storage (Blob/File) | Data + critical configs | Daily/weekly depending | 30–365 days | Use redundancy + lifecycle policies. |
| Key/Secrets | Secrets/keys/certs exports where allowed | After changes + scheduled | Per compliance | Protect exports; restrict access. |
| IaC repos | Git repos + pipeline configs | Continuous | Long-term | Treat as rebuild source-of-truth. |

> Replace “example” frequency/retention with values approved by Compliance, Security, and Clinical leadership.

### 10.3 Backup immutability and separation
- Store backups with strong access controls and separation from production admin roles.
- Require MFA and privileged workflows to delete/alter backups.
- Implement “two-person rule” for destructive backup actions for Tier 0 systems.

---

<!-- PAGE 11 -->

## 11. Testing, exercises, and continuous improvement

### 11.1 Test types and cadence
- **Monthly:** Restore test of a Tier 1 system (proof of process).
- **Quarterly:** Tier 0 partial failover simulation (tabletop + technical validation).
- **Semi-annual:** Full regional failover drill for Tier 0 stack (staged, approved downtime window if needed).
- **After every major change:** Targeted restore/failover test for affected system.

### 11.2 What to measure (record in every drill)
- Start time, end time, and achieved RTO
- Achieved RPO (data loss measurement)
- Manual steps required (candidates for automation)
- Issues, root cause, and remediation owner/date

### 11.3 Change management integration
- No production go-live for Tier 0 systems without:
  - Updated runbook
  - Updated dependency mapping
  - Backup/restore validation
  - Rollback plan

---

<!-- PAGE 12 -->

## 12. Appendices (templates)

### 12.1 System record template (copy per system)
**System name:**  
**Tier:**  
**Owner:**  
**Primary Region resources:**  
**Secondary Region resources:**  
**Data stores:**  
**Failover method:**  
**RTO / RPO:**  
**Validation steps:**  
**Dependencies:**  
**Last DR test date / results:**  

### 12.2 Regional failover checklist (printable)
- [ ] Incident declared; roles assigned; bridge opened.
- [ ] Freeze deployments/changes.
- [ ] Confirm outage scope (region vs zone vs app-only).
- [ ] Confirm Secondary Region capacity and access.
- [ ] Execute traffic failover.
- [ ] Execute data failover per system.
- [ ] Scale application tiers.
- [ ] Validate Tier 0 clinical workflows with clinical liaison sign-off.
- [ ] Communicate status + limitations.
- [ ] Begin failback planning (do not rush).

### 12.3 Ransomware checklist (printable)
- [ ] Contain/isolate affected systems.
- [ ] Disable compromised identities; rotate secrets.
- [ ] Preserve logs/evidence.
- [ ] Stand up clean environment.
- [ ] Restore from known-good backups.
- [ ] Validate integrity + security checks.
- [ ] Reconnect integrations gradually.
- [ ] Communicate and document lessons learned.

### 12.4 Contact list placeholders
- On-call escalation: `__________`
- Cloud platform team: `__________`
- Security incident response: `__________`
- Clinical super-user group: `__________`
- Vendor support: `__________`

---

End of document.

If a specific primary/secondary Azure region pair, the EHR stack (VM-based vs PaaS), and the database technologies are provided, the runbooks can be rewritten with exact step-by-step commands and named Azure resources (vaults, failover groups, recovery plans, DNS records).

<div align="center">⁂</div>

[^1]: https://learn.microsoft.com/en-us/azure/reliability/regions-paired
