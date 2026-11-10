# Factory registry: 40+ factories, each registered once and reused by key

The backend that makes factories reusable: one database registry of factories, routes, runs and artifacts. Unambiguous agentic harnesses to avoid execution drift: each factory exposes named modes, so agents choose from a menu instead of improvising. Naming inspired by video game UI settings: clean and unambiguous.

## Results

| Measured result |
|---|
| **One registry base:** Factories, SubFactories, FactoryRoutes, FactoryRuns, FactoryJobs and FactoryArtifacts tables |
| **FactoryOps:** 40+ registration scripts |
| **FactoryOps:** a ranked name-to-factory resolver |
| **Factory UI spec:** 8 rules |
| **Factory UI spec:** zero free-text inputs |

## How it works

### FactoryOps

- Structure: Factories and SubFactories hold the reusable units; FactoryRoutes chains them into named routes with their steps; FactoryRuns, FactoryJobs and FactoryArtifacts record every run, job and output, so a run can be traced from request to file.
- Reuse by key: each factory is registered once with its key, purpose, aliases, skill, worker and start command, then reused by key. A new pipeline is a new route over existing pieces, not new code.
- Design philosophy: every factory has sub-modes that are totally unambiguous, for a clean-context agent or a human; one simple canonical way to execute each mode, with no back doors and no escape hatches. That is the approach across everything I build.
- One skill per factory: each factory ships with a dedicated skill that says exactly how to run and interact with it cleanly, so a fresh agent runs it the canonical way the first time.
- Resolver: a ranked lookup turns plain English ("which factory handles X?") into the best active, most recent route or sub-factory, with a one-line reason why it matches. Retrieval is the default; starting a run needs an explicit ask.
- Contracts: the demand side (the content and growth lanes) refers to factories only by stable keys for the route, run and job, so factories can be swapped or upgraded without breaking callers. Any route that writes to the post queue must go through one shared, gated writer.
- Lane executor: a skill turns a new repeated workflow into a lane packet and registers it, so one-off scripts get promoted into reusable factories.

### Factory UI spec

- Discrete options only, named presets (default/safe/aggressive/hero), units on every slider, fully serialisable state, named lanes, visible auto-dependencies and lock reasons, one-clause help.
- Link to the equations: each factory's axes become its controls, one discrete picker per axis, so the UI is the equation made playable.
- Result: zero ramp-up for subagents, identical re-runs, composable factories, reviewable by a non-engineer.

## What this covers

- **FactoryOps: one registry, every factory registered once and reused by key**: Every factory, sub-factory and route is registered in one database with its purpose, aliases, steps and runs, so any factory can be found by plain English and clicked into a bigger pipeline instead of rebuilt.
- **Factory UI spec: unambiguous agentic harnesses that avoid execution drift**: Unambiguous agentic harnesses to avoid execution drift: each factory exposes named modes, so agents choose from a menu instead of improvising. Naming inspired by video game UI settings: clean and unambiguous, with no context bloat.

---

Part of my [GitHub profile](../README.md), chapter: Backend and infrastructure.
