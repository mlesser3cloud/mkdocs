# Migration strategy

## Goal
Define a clear migration direction that balances **speed**, **risk**, and **value**, and produces an actionable plan for waves and workstreams.

## Outline
- Outcomes and principles
- The 6Rs (migration disposition decisions)
- Business case inputs and cost model approach
- Wave planning and sequencing
- Deliverables and checklists

## Outcomes and guiding principles
Start by agreeing on what “good” looks like.

### Define outcomes
- Business: time-to-market, reliability, regulatory posture, cost predictability
- Technical: reduced toil, standardized patterns, improved security baselines
- Delivery: repeatable waves, automation, fewer one-off exceptions

### Establish principles (examples)
- Prefer managed services where they reduce operational overhead (verify suitability per workload).
- Standardize identity/networking patterns; exceptions require explicit approval.
- Automate provisioning via Infrastructure as Code (IaC) where possible.
- Treat migration as a product: measure, improve, repeat.

## The 6Rs (disposition options)
Use the 6Rs to decide what happens to each workload. These are not purely technical decisions; they’re joint business/IT calls.

### 1) Rehost (lift and shift)
Move as-is to Azure IaaS (e.g., VMs). Fastest path, often highest carryover of technical debt.

Use when:
- Timeline pressure
- Minimal code change allowed
- You need a rapid exit from a datacenter

Watch for:
- Overprovisioning (sizing is often wrong)
- Licensing, supportability, and OS images
- Networking/security changes still required

### 2) Refactor (re-architect)
Change the architecture to cloud-native patterns.

Use when:
- You need scalability/resilience improvements
- The workload is strategic and long-lived

Watch for:
- Longer timelines; keep scope controlled
- Platform readiness (landing zone, CI/CD)

### 3) Replatform
Make targeted changes to reduce ops burden without full redesign (e.g., move from self-managed DB to a managed DB).

Use when:
- You can gain meaningful ops/cost improvements
- The app can tolerate moderate change

### 4) Repurchase (SaaS)
Replace with a SaaS product.

Use when:
- The workload is commodity
- Vendor provides acceptable feature parity and compliance

### 5) Retire
Decommission.

Use when:
- Low usage / duplicates / no longer required

### 6) Retain
Keep as-is (for now).

Use when:
- Regulatory or technical blockers
- Workload is near end-of-life

## Build a business case (pragmatic)
A business case should enable decisions, not become a months-long study.

### Inputs to capture
- Current run cost (infrastructure, licensing, support contracts, labor)
- Planned migration cost (tools, engineering effort, professional services)
- Cloud run cost estimate (compute, storage, network, managed services)
- Risk cost (outage impact, security exposure, compliance gaps)
- Value upside (agility, faster delivery, improved reliability)

### Cost model approach
- Start with **order-of-magnitude estimates** for the full portfolio.
- Build a **high-confidence model** for wave-1 and wave-2.
- Track assumptions explicitly (region, SKUs, reservations/savings plans, etc.).

> Note: Pricing and discount programs change. Validate assumptions in Azure pricing tools and official documentation.

## Wave planning and sequencing
Plan migration in waves to reduce risk and build repeatability.

### Categorize by complexity and risk
Create a simple 2×2 to pick early candidates:

- Low complexity / low risk: best for wave-1
- High complexity / high risk: defer until the factory is stable

Suggested attributes:
- Criticality (revenue, customer impact)
- Change appetite and allowed downtime
- Dependency count (upstream/downstream)
- Data sensitivity and compliance requirements
- Technology fitness (EOL OS/DB, unsupported frameworks)

### Define waves
For each wave, produce:
- Scope (apps/servers/databases)
- Migration approach (6R decision per item)
- Environment targets (subscriptions, VNets, resource groups)
- Timeline and change windows
- Test and validation requirements

### Workstreams (minimum)
- Platform (landing zone)
- Network and connectivity
- Identity and access
- Security and governance
- Migration execution (servers, databases)
- App remediation and testing
- Cutover and communications
- Operations handoff

## Deliverables
- Portfolio list with 6R disposition
- Wave plan and sequencing
- Business case summary and assumptions
- Migration principles and standards
- RAID log (risks, assumptions, issues, dependencies)

## Strategy checklist
- [ ] Outcomes and success metrics agreed
- [ ] Portfolio scope defined and owned
- [ ] 6R disposition recorded for top-priority workloads
- [ ] Wave-1 candidates selected with clear rationale
- [ ] High-level business case created with documented assumptions
- [ ] Landing zone readiness plan created (owners + timeline)

Next: [Assessment](../assessment/index.md)
