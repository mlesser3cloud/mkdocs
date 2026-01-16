# FinOps

FinOps makes cloud spend measurable and actionable across the organization.

## Goal
Establish visibility, accountability, and optimization practices that maximize business value from Azure spend during migration and ongoing operations.

## Outline
- FinOps lifecycle and principles
- Cost visibility and allocation
- Budgets, forecasting, and anomaly detection
- Commitment-based discounts
- Right-sizing and waste elimination
- FinOps culture and accountability
- Migration-specific guidance
- FinOps checklist

## FinOps lifecycle and principles

The FinOps Foundation defines three iterative phases:

**Inform**: Build visibility into cloud costs and usage  
**Optimize**: Reduce waste and improve unit economics  
**Operate**: Define policies, governance, and continuous improvement

!!! tip "Start small and iterate"
    Perfect cost allocation is not required day one. Begin with high-level visibility and refine as you learn which dimensions matter most to your organization.

## Cost visibility and allocation

### Tagging strategy
Tags drive allocation, chargeback, and accountability.

**Baseline tags**:

- **Owner** / Team: Who owns the workload
- **Environment**: dev, test, staging, prod
- **Cost Center** / Business Unit: For financial allocation
- **Application** / Workload: Logical grouping
- **Project** / Wave: For migration tracking

!!! note "Enforce tags at creation time"
    Use Azure Policy to require tags on resource groups and resources. Tag inheritance from resource groups simplifies compliance.

### Cost allocation methods
- **Chargeback**: Business units pay actual costs (drives accountability)
- **Showback**: Business units see costs but don't pay directly (visibility without billing friction)
- **Shared cost allocation**: Distribute shared platform costs (networking, management tools) proportionally

### Azure Cost Management + Billing
**Key capabilities**:

- Cost analysis with custom views and grouping (by tag, resource type, subscription)
- Exports to storage for custom reporting and data warehousing
- Power BI integration for executive dashboards
- Cost allocation rules for distributing shared costs
- Recommendations for reservations, right-sizing, and idle resources

> Cost Management capabilities evolve frequently. Verify current features and any enterprise agreement-specific options with your Microsoft account team.

## Budgets, forecasting, and anomaly detection

### Budget strategy
Define budgets at multiple scopes:

- **Subscription level**: Prevent runaway spend per team/workload
- **Resource group level**: Track per-application or per-wave costs
- **Tag-based budgets**: Cross-subscription cost tracking (e.g., all "prod" resources)

**Alert thresholds**:

- 50%: Informational, track trends
- 80%: Warning, review forecast
- 100%: Critical, investigate immediately
- 110%+: Breach threshold with escalation

### Forecasting
Use Azure Cost Management forecasts to:

- Predict month-end spend based on historical trends
- Model impact of planned changes (new resources, commitment purchases)
- Identify seasonal patterns in usage

!!! tip "Validate forecasts during migration"
    Migration creates atypical usage patterns. Manually adjust forecasts during active migration waves and recalibrate after stabilization.

### Anomaly detection
Enable anomaly alerts to catch:

- Unexpected resource creation (rogue deployments, crypto mining)
- Configuration changes that spike costs (VM size increases, premium storage)
- Runaway autoscaling or data transfer

## Commitment-based discounts

Reduce on-demand costs through commitments:

### Reserved Instances (RIs)
- 1- or 3-year commitment for specific VM families and regions
- Up to 72% savings vs. on-demand
- **Use for**: Stable, predictable workloads with known capacity

### Azure Savings Plans
- 1- or 3-year commitment to a dollar amount per hour
- Flexibility across VM families, regions, and compute services
- Up to 65% savings vs. on-demand
- **Use for**: Workloads with variable shapes or multi-region deployments

### Spot VMs
- Up to 90% savings vs. on-demand
- Can be evicted with 30-second notice
- **Use for**: Fault-tolerant batch jobs, dev/test, stateless scale-out

**Commitment best practices**:

- Start conservatively (30-50% coverage) and increase over time
- Analyze 30-90 day usage trends before purchasing
- Use Savings Plans for flexibility unless RIs offer materially better rates
- Review and adjust quarterly as workloads stabilize

!!! warning "Avoid premature commitments during migration"
    Wait until workloads stabilize post-migration to understand true steady-state usage. Temporary migration infrastructure should use on-demand or Spot pricing.

## Right-sizing and waste elimination

### Continuous optimization
Optimization is ongoing, not one-time:

- **Right-sizing**: Match resource size to actual utilization (CPU, memory, IOPS)
- **Idle resources**: Delete or deallocate unused VMs, disks, IPs, and storage accounts
- **Orphaned resources**: Clean up disks detached from VMs, unused NICs, old snapshots
- **Autoscaling**: Enable where workload patterns justify it (web apps, batch processing)

### Common waste patterns
- Oversized VMs running at <20% CPU/memory
- Stopped VMs with attached premium disks (storage costs continue)
- Public IPs not associated with running resources
- Dev/test environments running 24/7
- Unattached managed disks and snapshots beyond retention policy

### Azure Advisor recommendations
Leverage built-in Advisor recommendations for:

- VM right-sizing based on utilization metrics
- Reserved Instance and Savings Plan purchase recommendations
- Idle and underutilized resources

**Act on recommendations systematically**:

- Review weekly during migration, monthly post-migration
- Track acceptance/rejection reasons for auditing
- Automate low-risk actions (delete old snapshots, deallocate idle VMs)

## FinOps culture and accountability

### Organizational structure
**Establish clear roles**:

- **FinOps team**: Central visibility, tooling, reporting, and recommendations
- **Engineering teams**: Accountable for workload costs, respond to recommendations
- **Finance/procurement**: Budget planning, invoice reconciliation, vendor management
- **Executives**: Set cost targets, approve major commitments

See [RACI](../reference/raci.md) for sample responsibility assignments.

### Ceremonies and cadence
- **Weekly wave review**: Cost tracking per migration wave (during migration)
- **Monthly FinOps review**: Trend analysis, anomalies, optimization backlog
- **Quarterly business review**: Forecast vs. actual, commitment renewals, strategic decisions

### Metrics and KPIs
Track and report on:

- Total cloud spend and trend (month-over-month, year-over-year)
- Unit economics (cost per transaction, per user, per GB processed)
- Commitment coverage % and utilization %
- Optimization savings realized (right-sizing, waste elimination)
- Budget adherence by team/workload

!!! tip "Make cost visible to engineers"
    Integrate cost data into engineering workflows (dashboards, pull requests, sprint reviews). Engineers optimize what they measure.

## Migration-specific guidance

### Expect temporary cost increases
- **Double-run costs**: Running both on-premises and Azure during cutover periods
- **Data transfer**: Egress and ingress during replication and migration
- **Temporary infrastructure**: Jump boxes, migration tools, extra environments for validation

**Mitigation**:

- Plan migration waves to minimize overlap and double-run duration
- Use efficient replication methods (Azure Migrate, Storage Migration Service)
- Deallocate migration infrastructure immediately after cutover

### Track cost by wave
- Tag resources with wave/project identifiers
- Create wave-specific budgets and cost views
- Compare actual costs to wave estimates to validate sizing assumptions

### Optimize after stabilization
**Avoid premature optimization**:

- Migrated workloads often run initially with "lift-and-shift" sizing
- Allow 30-60 days of operational data before aggressive right-sizing
- Prioritize stability and reliability over immediate cost reduction

**Post-stabilization optimization backlog**:

- Right-size VMs based on actual utilization
- Convert premium storage to standard where IOPS requirements allow
- Implement autoscaling for variable workloads
- Purchase commitments for steady-state resources

Related: [Operations](../operations/index.md) for cost management practices in day 2 operations.

## FinOps checklist

**Visibility and allocation**:

- [ ] Tagging standard defined and enforced via Azure Policy
- [ ] Cost allocation method chosen (chargeback vs. showback)
- [ ] Azure Cost Management dashboards created for key stakeholders
- [ ] Cost exports configured for data warehouse / BI integration

**Budgets and alerts**:

- [ ] Budgets defined at subscription, resource group, and tag levels
- [ ] Alert thresholds configured (50%, 80%, 100%, 110%+)
- [ ] Anomaly detection enabled and routed to FinOps team
- [ ] Forecast accuracy reviewed monthly and adjusted

**Optimization**:

- [ ] Azure Advisor recommendations reviewed weekly (during migration) or monthly (post-migration)
- [ ] Right-sizing analysis performed on migrated workloads post-stabilization
- [ ] Idle and orphaned resource cleanup automated or scheduled
- [ ] Commitment-based discount strategy defined (RIs, Savings Plans, Spot)
- [ ] Commitment coverage and utilization tracked quarterly

**Culture and accountability**:

- [ ] FinOps roles and responsibilities documented (see [RACI](../reference/raci.md))
- [ ] Monthly FinOps review ceremony scheduled with action tracking
- [ ] Cost visibility integrated into engineering workflows
- [ ] Unit economics and KPIs defined and reported

**Migration-specific**:

- [ ] Wave-based cost tracking and budgets configured
- [ ] Double-run cost estimates included in migration planning
- [ ] Post-migration optimization backlog prioritized and scheduled
- [ ] Temporary migration infrastructure identified for cleanup

Related:

- [Governance](../governance/index.md) for broader migration governance practices
- [Operations](../operations/index.md) for day 2 cost management
- [Reference: Checklists](../reference/checklists.md) for consolidated migration checklists
