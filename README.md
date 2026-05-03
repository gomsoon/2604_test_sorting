# 2604 Test Sorting

This repository is the starting point for a cross-platform desktop application built with Electron.

The current functional artifact is a single-file sorting visualizer in [index.html](./index.html). The broader goal is to evolve it into a structured desktop app that runs consistently on Windows, macOS, and Linux.

## Current Focus

- Keep the app easy to run while the architecture is still forming
- Document development rules before scaling the codebase
- Preserve a refactor-first, test-backed workflow
- Grow from a browser-friendly prototype into an Electron application

## Documentation Index

- [Architecture](./docs/260503_architecture.md)
- [Development Workflow](./docs/260503_development-workflow.md)
- [Dual Environment Workflow](./docs/260503_dual-environment-workflow.md)
- [Testing Strategy](./docs/260503_testing-strategy.md)
- [Initial Stack Decision](./docs/260503_initial-stack-decision.md)

## Working Principles

- Perform a structural review before adding meaningful new functionality
- Refactor first when the current structure would make the next change risky or expensive
- Keep logic separated from UI code so it can be tested directly
- Run regression tests after every non-trivial change
- Track branch coverage and use boundary-focused test cases for core logic
- Keep the repository safe for both Linux and Windows development through Git-based synchronization

## Near-Term Direction

1. Wrap the existing visualizer in a minimal Electron shell
2. Split responsibilities across `main`, `preload`, `renderer`, and pure domain logic
3. Introduce a test stack before the app grows too large
4. Add packaging and distribution concerns only after local development flow is stable

## Current Run Mode

The project can now be run in two lightweight ways:

- open [index.html](./index.html) directly in a browser
- run the Electron shell locally after installing dependencies

## Local Development

Install dependencies:

```bash
npm install
```

Start the Electron app:

```bash
npm start
```

The initial Electron bootstrap keeps the current single-file visualizer intact and loads it through `main.js` with a minimal `preload.js` bridge.
