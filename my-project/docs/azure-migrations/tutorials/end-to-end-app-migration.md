# Tutorial: End-to-end app migration

This tutorial focuses on app migration readiness and safe cutover.

## Steps

1. Pick a target approach and record it in an ADR.
2. Externalize configuration and secrets.
3. Add health checks and observability.
4. Deploy to non-prod and run performance validation.
5. Cut over with explicit rollback triggers.

## Verification

- Smoke tests pass.
- Key SLO indicators remain stable.
- Top failure modes have documented runbooks.
