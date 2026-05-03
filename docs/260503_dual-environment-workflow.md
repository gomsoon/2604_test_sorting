# Dual Environment Workflow

## Purpose

This document defines how the project should be developed across multiple local environments, starting with Linux Codex and Windows Codex, while using GitHub as the synchronization point.

## Source of Truth

GitHub should be treated as the primary synchronization source across environments.

The initial implementation sequence for this project is:

1. bootstrap and validate the Electron shell on Linux
2. push the working baseline to GitHub
3. pull and verify the same baseline on Windows
4. continue feature work with small synchronized changes

The recommended flow is:

1. pull before starting work
2. make a small, focused change
3. run the relevant tests
4. commit and push promptly
5. pull again before continuing from another machine

## Repository Safety Rules

- do not commit generated dependencies such as `node_modules`
- do not commit virtual environments
- do not commit local coverage artifacts, build output, or OS-specific cache files
- keep line endings normalized through repository settings rather than editor guesswork

## Branching Guidance

- use `main` as the stable integration branch unless a different team rule is introduced later
- create short-lived feature branches for non-trivial changes
- rebase or merge frequently enough to avoid long-lived divergence
- do not let Linux and Windows work drift independently for long periods

## Cross-Platform Editing Guidance

- prefer UTF-8 text files
- keep repository text files on LF by default
- allow Windows-native script files such as `.bat` and `.cmd` to use CRLF when needed
- avoid OS-specific shell commands in project scripts when cross-platform alternatives exist

## Development Discipline

Before starting work on a second machine:

- confirm that all local work on the first machine is committed or intentionally shelved
- pull the latest remote state
- review changed files before continuing implementation

After finishing work on either machine:

- run the relevant regression tests
- confirm no temporary local artifacts were added
- push the latest commit so the other environment can continue safely

## Smoke Testing Guidance

Run at least a light smoke check on both Linux and Windows when a change affects:

- file paths
- dialogs or filesystem interactions
- packaging behavior
- keyboard shortcuts
- window lifecycle behavior
- shell or process execution

## Conflict Handling Rule

If the same file needs substantial edits on both machines, prefer:

1. finishing one slice and pushing it first
2. pulling and integrating that slice on the other machine
3. continuing with the next isolated change

This is safer than allowing parallel large edits on the same code path.

## Why This Matters

Electron projects are cross-platform by nature, so development habits should reflect that early.

A disciplined GitHub-based workflow reduces merge pain, protects portability, and makes later packaging and release work much easier.
