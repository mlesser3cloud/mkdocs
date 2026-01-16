# Checklists

These checklists are designed to be copied into tickets, runbooks, or project plans.

## Strategy checklist
- [ ] Outcomes and success metrics defined
- [ ] Portfolio scope agreed and owned
- [ ] 6R disposition recorded for wave candidates
- [ ] Business case drafted with assumptions
- [ ] Wave plan created with sequencing rationale
- [ ] Risks and dependencies documented

## Assessment checklist
- [ ] Inventory complete for wave-1 (servers, DBs, owners)
- [ ] Dependency map created for wave-1 apps
- [ ] Performance baselines captured (CPU/mem/IOPS/latency)
- [ ] Target sizing assumptions documented
- [ ] Remediation backlog prioritized and staffed

## Landing zone checklist
- [ ] Subscription and management group structure defined
- [ ] RBAC model implemented using groups
- [ ] Privileged access process in place (JIT where possible)
- [ ] Network architecture approved (addressing, DNS, routing)
- [ ] Logging destination and retention defined
- [ ] Policy baseline applied (tags, regions, diagnostics where supported)
- [ ] IaC approach established and documented

## Server migration checklist
- [ ] Azure Migrate project created and access validated
- [ ] Discovery completed and grouped into waves
- [ ] Target network placement and DNS plan confirmed
- [ ] Replication enabled and healthy
- [ ] Pilot cutover completed successfully
- [ ] Wave runbook rehearsed and timed
- [ ] Monitoring and backup enabled post-cutover

## Database migration checklist
- [ ] Target pattern selected (IaaS/PaaS/refactor)
- [ ] Compatibility assessment completed
- [ ] Security model approved (auth, network, secrets)
- [ ] Migration method selected (offline/online)
- [ ] Validation plan prepared (data + app)
- [ ] Performance baseline and tuning plan defined
- [ ] Rollback plan tested

## Cutover checklist
- [ ] Go/no-go criteria defined
- [ ] Communications plan prepared and scheduled
- [ ] Change freeze communicated and enforced
- [ ] Final sync/cutover steps documented and rehearsed
- [ ] Validation tests ready (smoke + deep checks)
- [ ] Rollback triggers and steps agreed
- [ ] Hypercare staffing and schedule confirmed

## Operations checklist
- [ ] Dashboards and alerts in place (low noise)
- [ ] Backup policy defined and restore tests scheduled
- [ ] Patch strategy defined and executed
- [ ] Incident response process documented
- [ ] Cost budgets and alerts configured
- [ ] Post-migration optimization backlog maintained

Related:
- [Glossary](glossary.md)
