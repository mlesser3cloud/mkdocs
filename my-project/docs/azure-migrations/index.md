# Azure migrations

## Purpose
This documentation set helps you plan and execute an Azure migration end-to-end—from strategy and assessment through landing zone, migration execution, cutover, and day-2 operations.

Assumptions:

- You have an Azure subscription and can create resources (or can request them via your org’s process).
- You can access both Azure Portal and Azure CLI (`az`).
- You will validate any service-specific limits/requirements in official Azure documentation before finalizing designs.

## Outline (what’s in this suite)
1. **Strategy**: define outcomes, 6Rs decisions, business case, and wave plan.
2. **Assessment**: discover estate, map dependencies, and size target capacity.
3. **Landing zone**: establish platform foundations (identity, network, governance, ops).
4. **Migrate**: execute server and database migrations with repeatable runbooks.
5. **Cutover**: plan communications, change windows, rollback, and go-live.
6. **Operations**: monitoring, backup, patching, security, and cost management.
7. **Reference**: checklists and glossary.

## How to use these docs
Use this as a “stage gate” flow. Don’t try to perfect everything up-front—aim for **a repeatable, low-risk migration factory**.

Recommended reading order:

- Start with [Strategy](strategy/index.md)
- Then [Assessment](assessment/index.md)
- Build the [Landing zone](landing-zone/index.md)
- Execute [Server migrations](migrate/servers.md) and [Database migrations](migrate/databases.md)
- Run [Cutover](cutover/index.md)
- Stabilize with [Operations](operations/index.md)
- Use [Checklists](reference/checklists.md) during each phase

## Roles and responsibilities (RACI starter)
This suite assumes at least these roles (they can be combined in small teams):

- **Business owner / product owner**: defines outcomes, approves scope and downtime.
- **Program / project manager**: plans waves, tracks progress, runs comms.
- **Cloud platform team**: builds landing zone and guardrails.
- **App team**: remediates, tests, and owns app cutover.
- **DBA / data team**: plans database migration, validation, and performance.
- **Security**: reviews identity/networking controls and risk acceptance.
- **Operations/SRE**: defines monitoring/backup/patching and supports go-live.

## Stage gates (use as exit criteria)
Use these gates to decide if you’re ready to move forward.

### Strategy → Assessment
- [ ] Scope defined (apps, servers, databases)
- [ ] Migration goals and success metrics agreed
- [ ] Funding model and timeline agreed (at least at a high level)

### Assessment → Landing zone
- [ ] Inventory complete enough to plan first wave
- [ ] Dependencies identified for wave-1 apps
- [ ] Target architecture direction agreed (IaaS vs PaaS where feasible)

### Landing zone → Migrate
- [ ] Identity and networking baseline available
- [ ] Governance guardrails in place (policy/tags/logging)
- [ ] Operations baseline ready (monitoring, backup, access model)

### Migrate → Cutover
- [ ] Runbooks practiced in a test environment
- [ ] Performance and functional validation passed
- [ ] Cutover plan approved (including rollback)

### Cutover → Operations
- [ ] Monitoring and alerting tuned (noise reduced)
- [ ] Backup/DR validated
- [ ] Ownership and support model confirmed

## Quick-start commands (Azure CLI)
Use these to confirm your CLI environment and subscription context.

```bash
az version
az account show
az account list --output table
# Optional: select the right subscription
az account set --subscription "<SUBSCRIPTION_ID_OR_NAME>"
```

## References
- “Verify in Azure docs” applies to:
  - service availability by region
  - feature availability per SKU
  - supported migration paths and tooling prerequisites

Next: [Migration strategy](strategy/index.md)
