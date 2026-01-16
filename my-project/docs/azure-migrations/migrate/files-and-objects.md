# Migrate files & objects

File and object migrations are deceptively hard: bandwidth, millions of small files, and writer cutover semantics.

## Plan first

- What is the source of truth during migration (old vs new)?
- Will you run a period of dual-write? If not, how do you pause writers?
- What is your integrity and reconciliation approach?

## Validation checklist

- [ ] File counts and sizes match expected baselines
- [ ] Spot-check checksums for critical paths
- [ ] Application read/write validation completed
- [ ] Access control model validated (users/apps can access what they should)
