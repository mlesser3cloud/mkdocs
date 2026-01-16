# Risk register

Track risks continuously; most migration risk is discovered during execution.

## Template

| Risk | Impact | Likelihood | Mitigation | Owner | Status |
|---|---:|---:|---|---|---|
| Unknown dependency causes outage | High | Medium | Dependency mapping + rehearsal | App | Open |

## Common risks (starter list)

- DNS cutover issues (TTL, caching, split-horizon)
- IP overlap with on-prem networks
- Missing ownership for legacy systems
- Underestimated data migration windows
