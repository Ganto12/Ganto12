# Factory registry: every factory a Lego piece, every UI a game

The backend that makes factories reusable: one database registry of factories, routes, runs and artifacts, the UI rules every factory follows, and the database-backed ops subsystems around them.

## Results

| Measured result |
|---|
| **One registry base:** Factories, SubFactories, FactoryRoutes, FactoryRuns, FactoryJobs and FactoryArtifacts tables |
| 40+ registration scripts |
| a ranked name-to-factory resolver |
| 8 rules |
| zero free-text inputs |
| 4 subsystems |
| AppOps has 12 tables |

## Method (in brief)

- Structure: Factories and SubFactories hold the reusable units; FactoryRoutes chains them into named routes with their steps; FactoryRuns, FactoryJobs and FactoryArtifacts record every run, job and output, so a run can be traced from request to file.
- Design philosophy: every factory has sub-modes that are totally unambiguous, for a clean-context agent or a human; one simple canonical way to execute each mode, with no back doors and no escape hatches. That is the approach across everything I build.

## What this covers

- **FactoryOps: a registry that makes every factory a Lego piece**: Every factory, sub-factory and route is registered in one database with its purpose, aliases, steps and runs, so any factory can be found by plain English and clicked into a bigger pipeline instead of rebuilt.
- **Factory UI spec: every factory is a video game**: Every factory ships like a video game: named modes and runnables with clean, easy names, so agents like Codex pick options instead of improvising, with no context bloat or confusing wording.
- **NocoDB-backed ops subsystems**: Databases that run the business side: demand, execution, apps and agent missions.

---

Part of my [GitHub profile](../README.md), chapter: Backend.
