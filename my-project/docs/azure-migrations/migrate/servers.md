# Server migration (Azure Migrate)

## Goal
Migrate on-premises or other-cloud servers to Azure with repeatable processes using Azure Migrate as the primary orchestration and tracking capability.

## Outline
- When to use server migration vs modernize
- Prerequisites and access
- Discovery and assessment workflow
- Migration execution (pilot → waves)
- Validation and optimization
- Runbook and checklist

## When to migrate servers as servers
Server migration is appropriate when:
- You need speed (rehost)
- The application is tightly coupled to the OS
- Refactoring is not currently feasible

Consider replatform/refactor when:
- Operational overhead is high
- Scalability/reliability requirements exceed current architecture
- Managed services can reduce risk and effort

## Prerequisites
### Platform prerequisites
- Landing zone ready: identity, networking, governance ([Landing zone](../landing-zone/index.md))
- Target subscription/resource group conventions defined
- Connectivity plan: DNS, outbound access, and any required inbound access

### Migration tool prerequisites
Exact prerequisites can change. Verify in official Azure Migrate documentation:
- supported source environments
- required permissions and network access
- agent vs agentless options
- replication requirements and supported OS configurations

### Access prerequisites
- Azure permissions to create Azure Migrate project and resources
- Source environment access to discover and replicate servers

## Workflow overview
### 1) Create an Azure Migrate project
Portal-first is common for initial setup.

- Azure Portal: search for **Azure Migrate** → create a project
- Choose subscription, resource group, and region

### 2) Discover and assess servers
Goals:
- inventory what exists
- understand utilization patterns
- group servers by application/wave

Key outputs:
- recommended sizing (validate)
- readiness flags (OS, configuration)
- dependency insights (as available)

### 3) Plan target configuration
For each server group:
- target region
- VM sizing approach (right-size based on baselines)
- disk mapping and performance expectations
- network placement (VNet/subnet)
- identity requirements (domain join, managed identity patterns)

### 4) Pilot migration
Pick a low-risk subset that still exercises the end-to-end path:
- one typical app stack
- representative networking and identity patterns
- includes at least one database dependency (even if migrated separately)

Pilot success criteria:
- replication works reliably
- cutover steps are predictable
- validation tests pass
- rollback is feasible

### 5) Execute waves
For each wave:
- freeze scope (no surprises)
- run replication health checks
- schedule change window
- run cutover and validation
- document learnings and update runbook

## Cutover considerations (server)
Coordinate with the broader [Cutover plan](../cutover/index.md).

Typical steps:
1. Change freeze / deploy freeze
2. Final delta sync (if applicable)
3. Cutover event (stop source workloads if required)
4. Start workloads in Azure
5. Update DNS/LB records and firewall rules
6. Validate functionality and performance

## Validation and optimization
### Validation
- App functional tests and smoke tests
- Connectivity tests (inbound/outbound)
- Identity/authentication tests
- Performance sanity checks

### Optimization
- Right-size based on observed Azure metrics
- Revisit disk performance and caching settings (verify current guidance)
- Enable backup and monitoring baselines
- Apply patching policy

## Runbook template (copy/paste)
Use this structure per wave.

### Pre-migration
- [ ] Confirm landing zone readiness (network, identity, policy)
- [ ] Confirm server inventory and owners for this wave
- [ ] Confirm required ports/flows and DNS plan
- [ ] Confirm backup/restore approach
- [ ] Confirm downtime window and communications

### Replication
- [ ] Replication enabled and healthy
- [ ] Test failover performed (non-prod)
- [ ] Performance baseline captured

### Cutover
- [ ] Change freeze enacted
- [ ] Final sync completed
- [ ] Cutover executed
- [ ] DNS/traffic updates applied

### Post-cutover
- [ ] App validation passed
- [ ] Monitoring/alerts enabled
- [ ] Backups enabled and tested
- [ ] Owners signed off

Next: [Database migration](databases.md)
