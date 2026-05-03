# Testing Strategy

## Purpose

This document defines how testing should support safe iteration in the Electron application.

The target is not just high coverage numbers. The target is reliable change detection, especially around process boundaries, state transitions, and user-visible behavior.

## Testing Layers

### Unit Tests

Use unit tests for:

- sorting algorithms
- random data generation rules
- boundary normalization
- state transition helpers
- pure utility functions

Unit tests should not require a running Electron app.

### Integration Tests

Use integration tests for:

- preload API wrappers
- IPC contract behavior
- input validation across boundaries
- filesystem adapters or other privileged service layers

Integration tests should verify that layers interact correctly without requiring full manual app operation.

### End-to-End Tests

Use E2E tests for:

- app launch
- first window rendering
- major user flows
- cross-layer behavior that must work from the user's point of view

E2E coverage should focus on a few high-value flows rather than trying to duplicate all unit coverage.

## Initial Tooling Direction

- unit and integration tests: `Vitest`
- Electron app behavior: `Playwright`
- coverage reporting: Vitest coverage with `v8` as the default starting point

This tool selection can be revisited if packaging or runtime constraints require it.

## Coverage Policy

- target global branch coverage: at least 80%
- prioritize strong branch coverage on core logic and boundary-handling code
- avoid chasing coverage numbers in animation-only UI code unless the behavior is critical

Where appropriate, raise per-file thresholds for high-risk logic modules.

## Boundary Value Analysis Rule

Boundary-focused test cases should be created for logic involving:

- minimum and maximum supported values
- empty or single-item collections
- already sorted input
- reverse-sorted input
- duplicate values
- invalid, missing, or malformed IPC payloads

Boundary tests are expected for core algorithms and any validation logic at process boundaries.

## Parallel Execution Rule

Parallel execution is encouraged when tests are independent.

- allow file-level parallelism for isolated unit and integration tests
- use worker configuration to reduce runtime where stability is preserved
- disable or limit parallelism for tests that share mutable global state or OS-level resources

Fast tests are important, but determinism is more important than raw speed.

## Failure Triage Order

When tests fail after a change:

1. confirm whether the failure is structural, behavioral, or environment-related
2. fix the root cause before adjusting assertions
3. revisit the architecture if the failure exposes poor boundaries
4. update tests only when the intended behavior truly changed

## Minimum Test Expectations By Change Type

- pure logic change: unit tests required
- IPC or preload change: integration tests required
- new user flow or desktop capability: E2E coverage required
- architecture refactor: regression tests for impacted layers required
