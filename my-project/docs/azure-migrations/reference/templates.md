# Templates

Use these as lightweight building blocks. Copy/paste into tickets, docs, or repos.

## Cutover run-of-show

- Change window:
- Incident bridge:
- Roles (change lead, comms lead, validation owners):
- Timeline:
  - T-60: freeze + backups
  - T-30: drain traffic / pause writers
  - T-0: switch + validate
  - T+30: stabilize + monitor
- Rollback triggers:

## Validation plan

- Functional smoke tests
- Data validation (row counts, checksums, reconciliation queries)
- Performance validation (baseline comparison)
- Security checks (logging, access, key rotation)

## Controls mapping (starter)

| Control | Evidence | Owner | Cadence |
|---|---|---|---|
| Central logging enabled | Log query + retention setting | Platform | Continuous |
| MFA required | Conditional access policy | Security | Quarterly |
