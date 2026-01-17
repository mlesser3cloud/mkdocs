# Migrate (overview)

Migration execution should run like a factory: consistent inputs, repeatable steps, and measurable outputs.

## Inputs

- Ownership and downtime tolerance confirmed
- Dependencies mapped (network, identity, data)
- Target design and sizing assumptions documented
- Cutover plan drafted with rollback triggers

## Repeatable stages

1. **Prepare**: landing zone prerequisites satisfied.
2. **Pilot**: migrate a low-risk workload to validate the factory.
3. **Waves**: migrate in batches with a stable runbook.
4. **Stabilize**: validate operational readiness.
5. **Optimize**: backlog for modernization and cost improvements.

## Workload types

- [Servers](servers.md)
- [Databases](databases.md)
- [Apps](apps.md)
- [Files & objects](files-and-objects.md)
- [HPC](hpc.md)
