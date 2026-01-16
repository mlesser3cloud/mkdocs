# Azure Migrations Playbook

This site is a practical, opinionated documentation suite for migrating workloads to Microsoft Azure.

[Start here](azure-migrations/index.md){ .md-button .md-button--primary }
[View checklists](azure-migrations/reference/checklists.md){ .md-button }

## Where to start

- Start here: [Azure migrations](azure-migrations/index.md)
- Building foundations: [Landing zone](azure-migrations/landing-zone/index.md)
- Migrating workloads: [Migrate](azure-migrations/migrate/index.md) → [Cutover](azure-migrations/cutover/index.md)
- Running day-2: [Operations](azure-migrations/operations/index.md)

## What you’ll find here

<div class="grid cards" markdown>

-   **Strategy & governance**

    Define target outcomes, decision records, and guardrails.

    [Strategy](azure-migrations/strategy/index.md){ .md-button }

-   **Landing zone foundations**

    Build identity, networking, security, and management baselines.

    [Landing zone](azure-migrations/landing-zone/index.md){ .md-button }

-   **Migration execution**

    Run repeatable waves with clear acceptance criteria and cutover plans.

    [Migrate](azure-migrations/migrate/index.md){ .md-button }

-   **Operations & resilience**

    Make day-2 operable: monitoring, backup, patching, and BC/DR.

    [Operations](azure-migrations/operations/index.md){ .md-button }

</div>

## What this playbook optimizes for

- Repeatable delivery (waves, runbooks, quality gates)
- Security and governance by default
- Clear cutover/rollback plans
- Post-migration operational readiness (monitoring, backup, patching)

!!! note
    This playbook avoids hard-coding Azure product limits that can change. When precision matters, it calls out what to verify in official Azure documentation.
