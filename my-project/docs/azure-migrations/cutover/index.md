# Cutover

## Goal
Execute go-live with controlled risk: clear communications, validated runbooks, defined rollback, and measurable success criteria.

## Outline
- Cutover readiness
- Change window planning
- Communications plan
- Execution timeline
- Rollback strategy
- Post-cutover stabilization
- Cutover checklist

## Cutover readiness (go/no-go)
A cutover should be boring. If it feels improvised, you are not ready.

### Readiness signals
- Migration runbooks rehearsed at least once in a non-prod or pilot environment
- Dependencies documented and confirmed (firewalls, DNS, identity)
- Monitoring and backup are enabled and verified
- Stakeholders agree on downtime and customer impact

### Define success criteria
Examples:
- app passes smoke tests and critical user journeys
- error rate stays below agreed threshold
- performance is within acceptable range
- support team confirms alerting is actionable

## Change window planning
### Choose the window
- Align to business low-traffic periods
- Ensure team availability (application, platform, network, security, DBA)
- Book escalation contacts and vendor support if needed

### Freeze policy
Define and communicate:
- code freeze date/time
- data/schema freeze requirements
- emergency change exceptions process

## Communications plan
Create a clear and reusable template.

### Stakeholders
- business owners
- service desk / NOC
- affected user groups
- downstream/upstream integration owners

### Communication cadence
- T-7 days: announce planned window
- T-24 hours: reminder with impact details
- T-0: start of window + status updates
- T+X: completion notice

### Message template (adapt)
Include:
- what is changing
- expected impact and downtime
- how to get help
- status page or channel

## Execution timeline (example)
Build a minute-by-minute runbook for wave cutover.

1. **Start**: declare window open, confirm roles on bridge
2. **Pre-checks**: validate replication health, backups, monitoring
3. **Freeze**: stop writes/deployments as required
4. **Final sync**: complete last delta sync
5. **Traffic switch**: DNS/LB/app config updates
6. **Validation**: smoke tests, key workflows, monitoring check
7. **Sign-off**: business confirms
8. **Close**: document outcomes and next steps

## Rollback strategy
Rollback is not a wish; it’s a designed capability.

### Decide rollback triggers
Examples:
- critical workflow fails and cannot be fixed quickly
- data inconsistency detected
- latency/error rates exceed agreed limits

### Rollback options
Options depend on architecture; validate feasibility in your environment.

- revert DNS/traffic routing to the old environment
- restore from backups/snapshots
- re-enable old services and disable new ones

### Rollback runbook requirements
- clear decision authority (who calls rollback)
- time limit for “fix forward” before rollback
- exact steps and owners
- validation steps after rollback

## Post-cutover stabilization
### Hypercare period
- dedicated on-call coverage
- reduced change rate
- daily review of incidents, alerts, and customer feedback

### Handoff to operations
- confirm monitoring dashboards and alerts
- confirm backup jobs and restore tests
- confirm patching ownership
- update documentation and CMDB (if used)

## Cutover checklist
- [ ] Go/no-go criteria defined and approved
- [ ] Runbooks rehearsed and timings measured
- [ ] Stakeholder comms drafted and scheduled
- [ ] Change freeze communicated
- [ ] Rollback plan tested (at least tabletop + key technical steps)
- [ ] Validation tests prepared (smoke + deep checks)
- [ ] Hypercare plan staffed

Next: [Operations](../operations/index.md)
