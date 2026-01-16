# Resilience (BC/DR)

Resilience is workload-specific. Don’t overbuild for low-criticality systems; don’t underbuild for crown jewels.

## Define targets

- $\textbf{RTO}$: maximum time to restore service
- $\textbf{RPO}$: maximum acceptable data loss window

## Validate routinely

- Backup restores are tested (not just configured)
- Failover/fallback procedures are rehearsed
- Dependencies are included (DNS, identity, third-party services)

## Example plan

- [Hell's Pass Hospital — Azure BCDR Failover Plan (24x7)](bcdr-failover-plan.md)
