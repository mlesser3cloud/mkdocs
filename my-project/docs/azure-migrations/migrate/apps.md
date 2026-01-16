# Migrate apps

Apps fail during migration when configuration, identity, and dependencies aren’t treated as first-class.

## Migration readiness checklist

- [ ] All configuration externalized (no hard-coded endpoints)
- [ ] Secrets are stored in a secret store (no secrets in repos)
- [ ] Health checks exist (readiness + liveness)
- [ ] Logging has correlation IDs (trace a request end-to-end)
- [ ] Dependency list is complete and tested from Azure networks

## Cutover patterns

- DNS-based switching (watch TTL and caching)
- Load balancer traffic shift / blue-green
- Canary release (if platform supports it)

Related:

- [Cutover](../cutover/index.md)
- [Operations](../operations/index.md)
