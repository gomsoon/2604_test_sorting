# Development Workflow

## Goal

This workflow is the default procedure for feature work in this repository.

It is designed to preserve code clarity while the project grows from a single-file prototype into a structured Electron application.

## Default Change Sequence

1. Review the current structure before adding behavior.
2. Decide whether refactoring is required first.
3. Narrow the scope of the next change.
4. Add or update tests for the affected logic.
5. Implement the change in small, reviewable steps.
6. Run regression tests and coverage checks.
7. Update docs when architecture, workflow, or decisions change.

## Structural Review Checklist

Before implementing a feature, review the relevant code for:

- mixed responsibilities
- hidden coupling across modules
- duplicated logic
- hard-to-test UI-bound logic
- missing validation at process boundaries
- unclear state transitions

If the answer to any of these is "yes" and the issue would make the next feature risky, refactor first.

## Refactor-First Rule

Refactoring should happen before feature work when:

- the target code is difficult to test safely
- the next feature would deepen existing coupling
- logic that belongs in a shared or domain module is trapped inside the UI layer
- the change would otherwise require fragile copy-paste edits

Refactoring should be incremental. Avoid broad rewrites unless the existing structure blocks progress.

## Feature Implementation Rule

When implementing a feature:

- keep desktop concerns out of domain logic
- keep domain logic out of `main` and `preload` where possible
- prefer small API surfaces between layers
- separate behavioral changes from visual polish when practical

## Regression Testing Rule

Every non-trivial code change must be followed by regression testing appropriate to its layer:

- domain logic: unit tests
- preload and IPC behavior: integration tests
- end-user behavior: E2E tests

Manual verification is useful, but it does not replace repeatable automated regression tests.

## Cross-Environment Rule

When switching between Linux and Windows development environments:

- pull the latest changes before starting work
- avoid large overlapping edits on the same files across environments
- commit and push small slices frequently
- rerun the relevant automated tests after pulling cross-environment changes
- perform at least a light smoke check when a change may behave differently by OS

## Definition of Done

A change is considered done when:

- the structure still reflects the architecture rules
- the intended behavior is implemented
- the affected tests pass
- coverage expectations are still met
- documentation is updated if the change affects team conventions or design decisions

## Change Size Guidance

Prefer small slices:

- one refactor
- one behavior change
- one related test set

This makes failures easier to diagnose and keeps structural intent visible.
