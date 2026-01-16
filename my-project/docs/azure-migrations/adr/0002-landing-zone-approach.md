# ADR 0002: Landing zone approach

## Status

Proposed

## Context

Ad-hoc subscriptions lead to inconsistent security, networking, and operations.

## Decision

We will implement a standard landing zone baseline with IaC, policy guardrails, and centralized logging before migrating production workloads.

## Consequences

- Upfront platform work is required
- Subsequent migrations become faster and less risky
