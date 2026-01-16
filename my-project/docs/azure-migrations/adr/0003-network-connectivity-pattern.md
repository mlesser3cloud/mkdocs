# ADR 0003: Network connectivity pattern

## Status

Proposed

## Context

Workloads require predictable connectivity and name resolution across environments.

## Decision

We will standardize on a hub-and-spoke network pattern with centralized egress controls and a defined DNS resolver strategy.

## Consequences

- Simplifies dependency mapping and troubleshooting
- Requires careful IP planning
