# Operations (day 2)

## Goal
Operate migrated workloads reliably and cost-effectively with clear monitoring, backup, patching, security, and cost management practices.

## Outline
- Observability and monitoring
- Backup and recovery
- Patching and vulnerability management
- Incident response and SRE practices
- Cost management
- Operations checklist

## Observability and monitoring
### What to monitor
- Availability (synthetic + real user where possible)
- Latency and error rates (app and dependencies)
- Resource health (CPU/memory/disk/network)
- Capacity trends and saturation
- Security signals (auth anomalies, privileged changes)

### Baseline practices
- Standard dashboards per workload
- Alert thresholds tuned to reduce noise
- Runbooks linked from alerts
- On-call ownership defined

> Monitoring capabilities vary by resource type. Verify current telemetry options and recommended collection settings in Azure docs.

## Backup and recovery
### Define backup policy
- Scope: VMs, databases, file shares, critical config
- Retention: daily/weekly/monthly requirements
- RTO/RPO targets and how they’re achieved

### Validate restores
A backup is only useful if restores are practiced.

- schedule regular restore tests
- document restore steps and access prerequisites
- verify application-level recovery, not just data restore

## Patching and vulnerability management
### Patch strategy
- Define patch windows per environment
- Use staged rollout: dev → test → prod
- Record exceptions with expiration dates

### Vulnerability management
- Track OS and dependency vulnerabilities
- Define remediation SLAs by severity
- Confirm who owns patching for each resource type

## Incident response and SRE practices
### Minimum incident process
- clear severity levels
- escalation paths
- post-incident review and action tracking

### Reliability improvements
- error budgets and service objectives (where applicable)
- reduce manual changes via automation
- capacity planning reviews

## Cost management
### Cost hygiene
- enforce tagging for chargeback/showback
- review idle/unused resources regularly
- right-size after migration (often significant savings)

### Spend controls
- budgets and alerts per subscription/resource group
- governance to limit uncontrolled resource sprawl

> Cost controls and discount programs change; verify available options in Azure docs and your enterprise agreements.

## Operations checklist
- [ ] Monitoring dashboards exist for each migrated workload
- [ ] Alerts are actionable with documented runbooks
- [ ] Backup policy defined and restore tests scheduled
- [ ] Patch windows defined and owned
- [ ] Incident process and on-call coverage confirmed
- [ ] Cost tags enforced and budgets configured
- [ ] Post-migration optimization backlog created (right-sizing, performance tuning)

Next: [Reference checklists](../reference/checklists.md)
