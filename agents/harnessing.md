# Agent harnessing: skills, clean-context agents and sustained output

How fleets of coding agents are run: one skill per job, clean-context agents instead of a central orchestrator, shortest lossless integration for every new module or adapter (the smallest change that plugs it in, with existing gates passing before and after), and the output that came from it.

## Results

| Measured result |
|---|
| 280 skills, 131 agentic, 110 factory-type (local scan, approx.) |
| 12+ orchestrator scripts → 4 overseers + 11 launchers |
| **~17,000 commits and ~162bn coding-agent tokens (mostly cached context) over 2025–26:** sustained daily output |
| Codex 119bn + Claude Code 42.9bn |

## Method (in brief)

- Method note: cached-prompt reads count toward totals but cost a fraction of normal tokens, so they are included in the totals but cost far less. Output tokens are a tiny share of the total.

## What this covers

- **Agent harnessing & frameworks**: How I run many agents in parallel without losing control, and add every new module or adapter by shortest lossless integration.
- **System evolution: central orchestrator → clean-context agents**: How my GPU and batch systems got simpler and more reliable as the agents improved.
- **Velocity & scale**: Consistent, aggressive output over time, measured from my GitHub profile and local logs.

---

Part of my [GitHub profile](../README.md), chapter: Agent harnesses.
