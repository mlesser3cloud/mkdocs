# Database migration (SQL patterns)

## Goal
Move SQL workloads to Azure using a pattern that matches your constraints (downtime, compatibility, scale, governance) while keeping validation and rollback practical.

## Outline
- Choose your target: IaaS vs managed
- Common SQL migration patterns
- Pre-migration readiness
- Execution runbook
- Validation and performance
- Checklist

## Choose your target
A practical framing:

- **Rehost**: move the database server/VM as-is (fastest, carries ops burden).
- **Replatform**: move to a managed database service where feasible (reduced ops).
- **Refactor**: change the data model/app behavior (largest change, potentially largest payoff).

> Service capabilities and constraints change frequently. Validate feature support, HA/DR options, and compatibility in official Azure docs before committing.

## Common SQL migration patterns
### Pattern A: Lift-and-shift the DB server (IaaS)
Use when:
- Minimal changes allowed
- You require OS-level control

Tradeoffs:
- You still patch/backup/manage the DB like today
- HA/DR remains your responsibility (unless you adopt platform options)

### Pattern B: Migrate to a managed SQL offering (PaaS)
Use when:
- You want to reduce patching/backup burden
- You can accept platform constraints

Tradeoffs:
- Compatibility review needed (features, jobs, linked servers, CLR, etc.)
- Requires governance alignment (network, identity, secrets)

### Pattern C: Hybrid (temporary)
Use when:
- App servers move first, DB stays on-prem temporarily
- You need phased risk reduction

Tradeoffs:
- Latency and bandwidth risks
- More complex security and troubleshooting

### Pattern D: Modernize with eventing/cache/search
Use when:
- Performance/scaling requirements demand redesign

Tradeoffs:
- More engineering, needs strong testing and SRE involvement

## Pre-migration readiness
### Inventory and classification
For each database:
- engine/version/edition
- size and growth rate
- HA/DR requirements (RTO/RPO)
- usage profile (OLTP vs reporting vs mixed)
- special features (jobs, replication, CDC, linked servers)

### Compatibility assessment
- Identify unsupported features for your chosen target
- Decide remediation options (replace, redesign, keep on IaaS)

### Security and access
- Define authentication approach (AAD/Entra, SQL auth, managed identities)
- Define secret storage and rotation
- Ensure firewall/private access patterns are approved (verify service options)

## Execution runbook (template)
Pick a migration approach that fits downtime constraints.

### Option 1: Offline migration (downtime window)
Best when you can tolerate downtime.

1. Freeze schema changes
2. Take backups/exports
3. Restore/import into Azure target
4. Run data validation
5. Switch app connection strings

### Option 2: Online/continuous replication (reduced downtime)
Best when downtime must be minimized.

1. Establish replication/continuous sync tooling
2. Validate ongoing sync health
3. Perform cutover during window (stop writes, final sync, switch)

> Exact tooling steps vary by database engine and target. Verify supported online migration methods in current Azure docs.

## Validation and performance
### Validation types
- Row counts and checksums where feasible
- Application functional tests
- Stored procedure/function test suite
- Performance tests for critical queries

### Performance tuning
- Establish baseline (before) and compare (after)
- Index maintenance and statistics updates
- Review parameterization and query plans
- Right-size compute and storage tiers based on observed load

## Database migration checklist
- [ ] Target pattern selected (IaaS/PaaS/refactor) and approved
- [ ] Compatibility gaps identified and remediations planned
- [ ] Security model defined (auth, network, secrets)
- [ ] Migration method selected (offline vs online)
- [ ] Validation plan defined (data + app)
- [ ] Rollback plan defined and tested

Next: [Cutover](../cutover/index.md)
