# Identity

Identity is the control plane for your migration: access, auditability, and blast radius.

## Outcomes

- Consistent human access with MFA/conditional access
- Least privilege for day-to-day operations
- Short-lived elevation for privileged operations
- Workload identity without long-lived secrets

## Recommended baseline (high level)

1. **Central tenant strategy**: confirm how many tenants you need and who owns them.
2. **Group-based RBAC**: assign roles to groups, not individuals.
3. **Privileged access**: use time-bound elevation (for example, PIM) for admin roles.
4. **Workload identity**: prefer managed identities over embedded credentials.
5. **Break-glass**: documented emergency access procedure and periodic testing.

!!! tip
    Treat identity changes like production changes: peer review, change windows for risky updates, and audit logging.

## Deliverables to produce

- Role model and naming standards (groups, service principals, managed identities)
- Access request flow (who approves what, and how long access lasts)
- Break-glass runbook

Related:

- [Security & compliance](../security/index.md)
- [Governance](../governance/index.md)
- [Reference: RACI](../reference/raci.md)
