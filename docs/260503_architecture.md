# Architecture

## Purpose

This document defines the target structure for the Electron-based application before full implementation begins.

The main goal is to keep desktop-specific responsibilities separate from rendering logic and to keep business logic testable without needing a running Electron window.

## Architectural Principles

- Separate platform concerns from application logic
- Prefer pure functions for domain behavior
- Keep renderer code focused on presentation and user interaction
- Use `preload` as the only bridge between the renderer and privileged Electron APIs
- Validate data at every process boundary
- Keep future Python integration optional rather than assumed

## Process Responsibilities

### Main Process

The main process is responsible for:

- application lifecycle
- window creation and configuration
- menus, dialogs, file system access, and OS integration
- IPC handlers for privileged actions

The main process should not contain renderer UI logic or algorithm visualization code.

### Preload

The preload layer is responsible for:

- exposing a minimal, safe API to the renderer
- wrapping IPC calls behind a stable browser-friendly interface
- shielding the renderer from direct Electron internals

The preload layer should stay thin. It is an interface boundary, not a business logic container.

### Renderer

The renderer is responsible for:

- rendering HTML/CSS UI
- handling user interaction
- driving animations and view updates
- calling preload-exposed APIs when desktop features are needed

Renderer code should avoid owning logic that can be extracted into reusable modules.

### Domain Logic

Domain logic is responsible for:

- sorting behavior
- data generation rules
- state transitions
- input validation
- reusable utility functions

This layer should be runnable and testable in isolation from Electron.

## Recommended Early Directory Shape

```text
project-root/
  index.html
  main.js
  preload.js
  src/
    renderer/
    domain/
    shared/
  tests/
    unit/
    integration/
    e2e/
  docs/
```

This does not need to exist on day one, but it is the intended direction once Electron bootstrapping starts.

## Dependency Rules

- `main` may depend on Electron and Node.js APIs
- `preload` may depend on Electron bridging APIs and lightweight validation helpers
- `renderer` may depend on browser APIs and shared presentation helpers
- `domain` must not depend on Electron APIs
- `domain` should avoid direct DOM access

## IPC Rules

- Define narrow, explicit channels
- Prefer request/response semantics for discrete actions
- Validate payload shape before performing side effects
- Return normalized success/error results instead of raw thrown internals where possible
- Do not expose unrestricted filesystem or shell access to the renderer

## Security Defaults

- Keep `contextIsolation` enabled
- Keep `nodeIntegration` disabled in renderer windows
- Expose only whitelisted APIs through `contextBridge`
- Treat all renderer inputs as untrusted

## Python Expansion Rule

Python or Flask should be introduced only when there is a clear functional need such as:

- specialized Python libraries
- heavy data processing
- AI or scientific workflows
- existing Python assets that would be expensive to rewrite

Until that need is concrete, the architecture should remain Electron-first with a clean option to add a sidecar process later.
