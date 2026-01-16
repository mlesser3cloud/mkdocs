# Networking

Standardized networking makes cutovers predictable.

## Design decisions to lock in early

- Connectivity pattern (hub/spoke vs alternatives)
- Ingress and egress control strategy
- DNS strategy (public/private zones and resolvers)
- IP plan (avoid overlap with on-prem)
- Segmentation model (subnets, NSGs, routing)

## Dependency mapping checklist

- [ ] Inventory all inbound endpoints (users, partners, APIs)
- [ ] Inventory outbound dependencies (SaaS, payment gateways, SMTP, etc.)
- [ ] Map east/west flows (service-to-service)
- [ ] Document name resolution dependencies (where does the app resolve DNS?)

!!! warning
    DNS is a common cutover failure mode. Validate private DNS resolution from workload subnets before migrating production.

Related:

- [Landing zone](../landing-zone/index.md)
- [Cutover](../cutover/index.md)
