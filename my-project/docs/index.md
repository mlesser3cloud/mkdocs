# Azure Migrations Playbook

This site is a practical, opinionated documentation suite for migrating workloads to Microsoft Azure.

## Where to start

- Start here: [Azure migrations](azure-migrations/index.md)
- Building foundations: [Landing zone](azure-migrations/landing-zone/index.md)
- Migrating workloads: [Migrate](azure-migrations/migrate/index.md) → [Cutover](azure-migrations/cutover/index.md)
- Running day-2: [Operations](azure-migrations/operations/index.md)

## What this playbook optimizes for

- Repeatable delivery (waves, runbooks, quality gates)
- Security and governance by default
- Clear cutover/rollback plans
- Post-migration operational readiness (monitoring, backup, patching)

!!! note
    This playbook avoids hard-coding Azure product limits that can change. When precision matters, it calls out what to verify in official Azure documentation.
