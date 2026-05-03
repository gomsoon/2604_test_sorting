# Initial Stack Decision

## Decision

Adopt Electron as the initial application shell and defer Python or Flask integration until a concrete need appears.

Decision date: 2026-05-03

Status: Accepted

## Context

The project goal is to build a desktop-style application that behaves consistently across Windows, macOS, and Linux.

The existing prototype is already HTML/CSS/JavaScript-based, which makes Electron a natural first step for turning the prototype into a desktop app.

There is also prior experience with Python and Flask, and those technologies remain viable for later expansion if the app needs them.

## Why This Decision Was Made

- the current prototype is already web-native
- Electron reduces the distance from prototype to desktop app
- starting with one runtime keeps setup and packaging simpler
- early focus should stay on structure, workflow, testing, and app boundaries
- Python can still be added later as a sidecar process if there is a real product need

## What This Means Now

In the initial phase:

- keep the app frontend-centered
- introduce Electron `main` and `preload` layers
- extract testable domain logic from renderer code
- establish the testing stack before the app becomes large

In the initial phase, do not:

- add Flask just to recreate local APIs that Electron can already handle
- add a second runtime without a concrete technical justification
- optimize packaging before the development workflow is stable

## Accepted Tradeoffs

- Electron adds desktop app structure and packaging concerns
- some future migration work may be needed if Python is later introduced
- process boundaries and IPC discipline are required from the start

These tradeoffs are acceptable because they support a cleaner first milestone and reduce unnecessary complexity early on.

## Revisit Triggers

Revisit this decision if:

- the project needs Python-only libraries
- algorithmic or data-processing workloads become heavy
- AI or scientific tooling is introduced
- local service-style APIs become clearly beneficial

If any of these conditions become important, a Python sidecar process can be evaluated at that time.
