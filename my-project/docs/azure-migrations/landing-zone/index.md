# Landing zone

## Goal
Create a secure, scalable Azure foundation aligned with the Cloud Adoption Framework (CAF) so migrations can proceed safely and repeatedly.

## Outline
- CAF alignment and design principles
- Identity and access
- Networking and connectivity
- Governance (policy, tags, standards)
- Security and logging
- Platform operations baseline
- Landing zone checklist

## CAF alignment (what to take from it)
CAF provides guidance across strategy, plan, ready (landing zone), adopt, and govern/manage. You don’t need every component to start—but you do need **a consistent baseline**.

Design principles:
- Standardize common patterns (networking, identity, logging)
- Separate duties (platform vs application ownership)
- Make security the default (least privilege, auditable access)
- Prefer automation (IaC, pipelines) over manual changes

> CAF reference architectures evolve. Validate current recommendations in Azure CAF documentation.

## 1) Identity and access
### Core decisions
- Tenant strategy (single vs multi-tenant, if applicable)
- Identity source (Entra ID, hybrid identity, domain services where needed)
- Access model: RBAC, privileged access, break-glass accounts

### Recommended baseline
- Define management groups/subscription structure (see governance)
- Establish role assignments by group, not by individual
- Require MFA and conditional access (verify policies with security team)
- Use just-in-time elevation for privileged roles when possible

### CLI/portal notes
- Portal is often easiest for initial tenant-level settings.
- Use CLI for repeatable role assignments and scripting.

Example RBAC assignment (illustrative):

```bash
# Assign a built-in role at a resource group scope
az role assignment create \
  --assignee-object-id "<GROUP_OBJECT_ID>" \
  --role "Reader" \
  --scope "/subscriptions/<SUB_ID>/resourceGroups/<RG_NAME>"
```

## 2) Networking and connectivity
### Core decisions
- Hub-and-spoke vs flat network (hub-spoke is common at scale)
- On-prem connectivity: VPN/ExpressRoute (verify options and prerequisites)
- IP addressing and DNS strategy
- Egress strategy (NAT, firewall, proxies)

### Baseline capabilities
- Standard VNet and subnet patterns
- Centralized DNS and name resolution approach
- Firewalling and routing standards
- Connectivity to on-prem (if required)

### Operational considerations
- Document allowed inbound/outbound flows
- Define how teams request new network rules (ticket + approvals)

## 3) Governance (policy, tags, standards)
### Structure
- Management group hierarchy for platform, landing zones, and workloads
- Subscription vending approach (manual at first, automated later)

### Policy baseline (examples)
- Require tags (cost center, owner, environment)
- Restrict regions to approved set
- Enforce diagnostic settings/logging where supported
- Restrict public exposure by default (verify service-specific capabilities)

> Policies differ per resource type and change over time; validate definitions in Azure Policy documentation.

## 4) Security and logging
### Logging baseline
- Central log destination (workspace/storage) and retention expectations
- Standard categories to collect: activity logs, resource logs, security logs
- Alerting rules: critical changes, admin role assignments, key vault access

### Key management
- Define secrets/certificates strategy (vault service, rotation practices)
- Decide how apps retrieve secrets (managed identity patterns where applicable)

## 5) Platform operations baseline
### Minimum day-0 operations
- Standard monitoring/alerting approach
- Backup standards and ownership
- Patch management approach (VMs and platforms)
- Incident response process (on-call, escalation, comms)

### Environment separation
- Define how dev/test/prod differ:
  - access controls
  - network segmentation
  - change controls
  - monitoring/backup retention

## Landing zone deliverables
- Management group + subscription structure
- Identity/access model and privileged access process
- Network architecture and connectivity plan
- Policy and tagging standards
- Logging and monitoring baseline
- Runbooks for provisioning and operations

## Landing zone checklist
- [ ] Subscription strategy and ownership documented
- [ ] RBAC model defined (groups, roles, scopes)
- [ ] Break-glass access defined and tested
- [ ] Network design approved (addressing, DNS, connectivity)
- [ ] Logging destination created and retention agreed
- [ ] Policy baseline applied (tags, regions, diagnostics where supported)
- [ ] Provisioning method defined (IaC preferred)

Next: [Server migration](../migrate/servers.md)
