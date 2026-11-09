# Architecture decision records

Accepted records capture agreed direction. Proposed records remain open for discussion.

## ADR-001: Make visuals and progression foundational

- Date: 2026-11-09
- Status: Accepted

### Context

Katascope prioritises a small number of engaging programming games. Visual feedback and a progression similar to Elevator Saga are central to its purpose.

### Decision

Include an engaging animated world and a sequence of challenges in the first playable version. Develop depth within a game before expanding the game catalogue.

### Consequences

Visual design and challenge design are part of the initial scope. An API-only simulation is an internal development step, not the first playable milestone. Challenges should encourage players to improve and carry forward their existing programs.

## ADR-002: Use an authoritative simulation with an external player API

- Date: 2026-11-09
- Status: Proposed

### Context

The project should run locally with a browser UI, support player programs in many languages, and leave a path to hosted sessions without executing participant code on a shared server.

### Decision proposal

Run an authoritative simulation behind a structured API. The browser provides visualisation and manual controls; player programs run separately and submit actions. Manual and programmed play use the same game rules and permitted observations.

Keep simulation rules independent of rendering and hosting. The simulation validates actions and computes transitions and outcomes. The renderer animates those outcomes.

### Consequences

The same game contract can support local and hosted operation. Language independence comes from the protocol, not the server's implementation language. Access control, validation, and resource limits remain necessary even without hosted code execution.

## ADR-003: Prefer a TypeScript stack with PixiJS

- Date: 2026-11-09
- Status: Proposed; technology selection is not yet agreed

### Context

The stack must support expressive 2D visuals, challenge progression, inspection, a local API, and eventual hosting while remaining manageable as a hobby project.

### Decision proposal

Use:

- TypeScript for an independent simulation module, frontend, and server.
- React for challenge selection, progression, controls, and inspection.
- PixiJS for the animated world, with Vite for frontend development and builds.
- Node.js and Fastify for the API and serving the built website.
- HTTP and JSON for actions and session controls; WebSocket for live browser updates.
- Versioned files for scenarios and challenge definitions.
- SQLite when durable progress, session history, and replay metadata are introduced.

### Alternatives considered

- React with SVG: strong fit for a crisp illustrated board-game style; more animation machinery to build ourselves.
- Phaser: more game presentation systems included, with additional framework conventions and potential overlap with simulation responsibilities.
- Svelte: an alternative to React for the surrounding UI.
- Go backend: attractive for distributing a standalone executable, at the cost of two implementation languages and less direct browser reuse of simulation code.
- Python with FastAPI: attractive for Python-led simulation development, with runtime packaging and cross-language contract maintenance to consider.

### Consequences

A shared simulation module can support server execution and potentially browser-only demonstrations. Rendering remains separate from game truth. Node.js distribution needs a runtime or later packaging work. Canvas-based visuals need accompanying HTML controls and accessible state information.
