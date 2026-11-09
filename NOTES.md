# Design notes

These are working ideas, not committed requirements unless recorded as accepted in [ADR.md](ADR.md).

## Origin and scope

The starting material is `~/code/teaming/GAMES.md`, a design exploration for team-programming workshop games. Katascope gives the games a separate home, with stronger emphasis on engaging visuals and built-in progression.

The intended learning loop is: explore manually, automate a strategy, watch it work, investigate failures, and improve it. Solo and small-team practice is a proposed initial focus; workshop facilitation can build on the same mechanics.

Hosting providers and pricing have not been selected. Recheck current terms when hosting becomes relevant.

## First game: passenger transport

Working name from the source document: **Boober**; public naming remains open.

Use a small ring of stops, passengers with origins and destinations, and vehicles with limited capacity. Begin with full visibility and a central fleet controller. Independent vehicle policies and partial observations can become advanced variants.

Proposed challenge progression:

| Challenge | Main learning opportunity |
| --- | --- |
| One vehicle and known requests | Observe, move, pick up, deliver |
| Requests arrive over time | Replace a fixed script with a repeating policy |
| Demand in both directions | Make routing choices |
| Limited capacity | Choose which work to serve |
| Two vehicles | Coordinate without duplicating effort |
| Uneven rush-hour demand | Distribute capacity |
| Maximum-wait objective | Prevent starvation rather than relying on averages |
| Announced road closure | Adapt an existing strategy |

Tune the sequence and thresholds through playtesting. Show delivered passengers, waiting passengers, average wait, and longest wait; avoid hiding trade-offs inside a single score.

## Second game: Elf Dispatch

A production-flow simulation with recipes, stations, limited buffers, stock, and deterministic processing durations. A possible starting scope is three stations and two recipes.

The central lesson is local efficiency versus whole-system flow. Later challenges can add bottlenecks, scarce components, rework, or complete-family-bundle objectives. A year-round factory theme could coexist with the Christmas presentation.

## Visual and interaction design

- Build a coherent visual style from the beginning: recognisable stops, distinct vehicles, visible capacity, passenger destinations, and clear waiting indicators.
- Animate movement, boarding, deliveries, and congestion so policy effects are understandable.
- Include pause, single-step, speed, restart, and replay controls.
- Manual controls submit the same legal actions available to programs. They support tutorials and experiments from paused states.
- Selecting an entity highlights its permitted observations and shows the corresponding structured data.
- Clearly distinguish any unrestricted spectator view from what a controller may observe.
- Explain rejected actions and failures in context; highlight the affected passenger or station and the relevant replay moment.
- Support reduced motion, keyboard controls, and labels or shapes alongside colour.

## Progression

- Distinguish games from challenges: one game can contain a substantial learning sequence.
- Give each challenge a clear objective, visible progress, understandable failure, and quick retry.
- Carry the player's program forward between challenges.
- Introduce one main difficulty at a time, then combine earlier ideas.
- Keep earlier challenges available for checking revised strategies.
- Save completion and best results locally without requiring an account.
- Recommend a sequence while allowing exploration, particularly for workshops.
- Keep scenario parameters, completion conditions, instructions, and prerequisites explicit and versioned.

## Simulation and protocol

- Separate rules, scenarios, sessions, and presentation without building a general-purpose game framework prematurely.
- Prefer deterministic tick-based transitions. Animation runs independently between simulation states.
- Proposed solo timing: wait for a controller decision, then advance the tick, with bounded timeouts and a visible stalled-controller state.
- Later live sessions need deadlines, a defined missing-action fallback, stale-action handling, reconnect rules, and simultaneous-action semantics.
- Replays need rules/scenario versions, initial conditions or seeds, accepted actions, and scheduled changes. A seed alone is insufficient.
- Stable scenarios and baseline policies help compare improvements; later evaluation can use several published seeds to discourage overfitting.

## Safety and bounded operation

- Keep player code on participants' machines; the hosted service accepts structured actions.
- Validate payload size, schema, session access, entity ownership, tick, and action legality. Clients cannot report authoritative scores or inventory changes.
- Bound rooms, entities, ticks, duration, connections, queued actions, request rates, logs, and replay retention.
- Stop idle sessions and require authorisation for hosted room creation and expensive operations.
- Bind the local service to loopback by default and protect state-changing requests from unrelated websites.
- Keep facilitator privileges and infrastructure credentials out of player programs.
- Application quotas do not guarantee a hosting bill ceiling. Evaluate infrastructure spending controls and define behaviour at capacity before public hosting.
- Browser-embedded code editing and execution are optional later features requiring a separate isolation design.

## Proposed first release

One complete visual transport game with a short challenge progression, manual exploration, a documented API, example policies, local progress, inspection, and replay. A single local launch starts the service and serves the browser UI.

Defer accounts, public leaderboards, hosted code execution, shared progress, and broad workshop administration.

## Open questions

- Confirm PixiJS versus SVG or Phaser, and the surrounding application stack.
- Choose visual style and an asset creation approach.
- Confirm the first game's name and precise movement, boarding, and routing rules.
- Define the smallest satisfying challenge sequence and completion thresholds.
- Decide controller state and reset semantics, API versioning, and a minimal cross-language example.
- Define first-release replay scope and which local data must persist.
- Decide whether a runtime-based launch is sufficient or a downloadable executable is important.
- Before hosting: expected concurrency, session length, acceptable interruptions, retention, and monthly budget.

## References

- [Elevator Saga](https://play.elevatorsaga.com/) and its [challenge definitions](https://github.com/magwo/elevatorsaga/blob/master/challenges.js).
- [PixiJS renderers](https://pixijs.com/8.x/guides/components/renderers).
- [SVG documentation](https://developer.mozilla.org/en-US/docs/Web/SVG).
- [Phaser scenes](https://docs.phaser.io/phaser/concepts/scenes).
- [Fastify validation](https://fastify.dev/docs/latest/Reference/Validation-and-Serialization/).
- [OWASP API resource consumption guidance](https://owasp.org/API-Security/editions/2023/en/0xa4-unrestricted-resource-consumption/).
