# Migration assessment

## Goal
Turn a portfolio list into an executable plan by discovering what you have, understanding dependencies, and sizing targets for Azure.

## Outline
- Discovery and inventory
- Dependency mapping
- Sizing and performance baselines
- Readiness and remediation backlog
- Outputs and checklists

## 1) Discovery and inventory
Start with a factual inventory; perfection is not required, but wave-1 needs high confidence.

### What to inventory
For each application or workload, capture:
- Owner/team and business criticality
- Environments (prod/non-prod) and regions/locations
- Servers/VMs and OS versions
- Databases and versions/editions
- Storage (file shares, object storage)
- External integrations (SaaS, partner systems)
- Authentication model (AD, SSO, service accounts)
- Backup/DR and RTO/RPO requirements

### How to discover (typical options)
- Existing CMDB or asset tooling
- Agent-based/agentless discovery tools
- Network scans and DNS/IPAM records
- Application team interviews and architecture reviews

> Tooling choices evolve; verify what is supported by the current Azure Migrate and related services in official docs.

## 2) Dependency mapping
Dependencies drive migration order and cutover risk.

### Dependency types
- Network: inbound/outbound ports, protocols, egress to internet
- App: API calls, message queues, batch jobs
- Data: DB connections, replication links, ETL pipelines
- Identity: domain joins, LDAP, SSO, certificate chains
- Shared services: SMTP, NTP, DNS, file shares

### Practical approach
1. **Start with wave-1 apps**: map their dependencies first.
2. Use a blend of:
   - observed network connections (where available)
   - architecture diagrams
   - app configs (connection strings, endpoints)
   - stakeholder workshops
3. Classify each dependency:
   - **Must move with** (tight coupling)
   - **Can stay** (loose coupling)
   - **Replace** (modernize or SaaS)

### Output format
- A dependency diagram per wave-1 app (simple is fine)
- A table listing critical endpoints and firewall rules needed post-move

## 3) Sizing and performance baselines
Sizing is a risk and cost lever. Avoid “copy the on-prem specs” unless you have evidence.

### Baseline metrics to capture
- CPU and memory utilization (p50/p95)
- Disk IOPS and latency
- Network throughput and burst patterns
- Peak business windows (month-end, promotions)

### Right-sizing principles
- Size to observed peaks + headroom, not to allocated capacity.
- Prefer smaller initial sizes with a plan to scale up if needed.
- Validate assumptions with performance tests where possible.

### Storage and performance considerations
- Map storage types (OS, data, logs, temp, backups)
- Identify latency-sensitive components (databases, log writers)

> VM families, disk SKUs, and performance characteristics change. Validate sizing guidance in the latest Azure docs.

## 4) Readiness and remediation backlog
Assessment should produce a prioritized backlog.

### Common remediation items
- Unsupported OS/DB versions (upgrade path)
- Hard-coded IPs or on-prem dependencies
- Legacy authentication flows
- TLS/certificate modernization
- Observability gaps (missing logs/metrics)
- Backup and DR redesign

### Readiness scoring (simple)
- **Green**: ready to migrate with minimal changes
- **Amber**: needs limited remediation
- **Red**: major blockers; defer or re-architect

## 5) Assessment outputs (minimum)
- Inventory with owners and criticality
- Dependency map for wave-1 and a method for ongoing mapping
- Sizing recommendations with assumptions
- Remediation backlog with priorities and owners
- Proposed target Azure services (IaaS/PaaS direction per workload)

## Assessment checklist
- [ ] Wave-1 inventory complete and validated with app owners
- [ ] Dependencies documented and reviewed by platform/network teams
- [ ] Performance baselines captured for critical workloads
- [ ] Sizing assumptions documented and approved
- [ ] Remediation backlog created and scheduled into waves

Next: [Landing zone](../landing-zone/index.md)
