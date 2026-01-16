# Tutorial: End-to-end landing zone bootstrap

This is an opinionated sequence for bootstrapping a landing zone without getting stuck in analysis paralysis.

## Steps

1. Confirm subscription strategy and target regions.
2. Establish identity model (groups, RBAC, privileged access).
3. Build network foundations (hub, DNS, egress controls).
4. Enable centralized logging and basic monitoring.
5. Apply governance guardrails (policies, tags, budgets).
6. Create a workload “starter” subscription/spoke.

## Verification

- A workload subscription can be created via automation.
- Logs from a test resource arrive centrally.
- Guardrails prevent unsafe defaults (missing tags, forbidden regions).
