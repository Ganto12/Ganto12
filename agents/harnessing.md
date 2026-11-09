# Agent harnessing: skills, clean-context agents and sustained output

How fleets of coding agents are run: one skill per job, clean-context agents instead of a central orchestrator, shortest lossless integration for every new module or adapter (the smallest change that plugs it in, with existing gates passing before and after), and the output that came from it.

## Results

| Measured result |
|---|
| **Agent harnessing & frameworks:** 280 skills, 131 agentic, 110 factory-type (local scan, approx.) |
| **System evolution:** 12+ orchestrator scripts → 4 overseers + 11 launchers |
| **~17,000 commits and ~162bn coding-agent tokens (mostly cached context) over 2025–26:** sustained daily output |
| **Velocity & scale:** Codex 119bn + Claude Code 42.9bn |

## How it works

### Agent harnessing & frameworks

- The harness around every factory: the equation sets what to make, gates check each cell against known-bad and known-good cases, and a clean-context auditor signs off before anything scales.
- Shortest lossless integration: the prefix on every agent task that adds a module, adapter or add-on to a complex system. Make the shortest change that plugs it in, and prove nothing that already worked was lost: the existing gates must pass before and after, and working code is extended, not rewritten. It is the cornerstone of how new pieces join a large system without breaking it.
- Single task register (P0–P2), one thread per task, clean-context auditors.
- Gold canonical evals with red/green checks against locked outputs.
- Cloning (Pabrai): copy what works, then improve it.

### System evolution

- Before: one central manager and orchestrator per workflow on rented Vast GPUs. Tidy-ish but redundant, bottlenecked and bug-prone. Two of the orchestrators (3,551 and 3,545 lines) differ by only about 68 lines.
- After: one canonical launcher per model that races 5–10 cheapest matching boxes and keeps the first that proves readiness; a short overseer skill (about 100 lines) that hands each wave to a clean-context subagent, waits about 30 minutes, retries up to 3 waves, caps live candidates at 10 and cleans up first.
- Around it: a registry as the single source of truth per workstream, fresh-context auditors, and guards that stop unsafe actions.
- Kill-tests: every hypothesis gets a cheap falsifier before anything is built, and bets that fail are killed early, so the loop spends build time only on ideas that survived.
- Model eras: this first ran with GPT 5.4, then GPT 5.6 "Luna" agents, and with Opus agents it became far easier to just kick off agents and guide them through batches.
- Reliability here is shown by design and structure; failure-rate numbers are not captured.

### Velocity & scale

- Commits: 17,004 commit contributions on my GitHub profile, almost all in private repos (492 in 2025 and 16,518 in 2026 so far). Earlier peaks from the old count: 849 commits in one day (2026-03-26).
- Tokens: measured floor of about 42.9bn tokens from Claude Code logs on my workstation alone (125k responses; about 41.6bn of that is cached-prompt reads, 0.09bn output). Codex, Kimi, MiniMax, OpenRouter and VM-side usage are not in that number.
- Codex: 119bn tokens, from the usage view in the ChatGPT app. Together with Claude Code's 42.9bn that is about 162bn tokens, and cached-prompt reads dominate it.
- Method note: cached-prompt reads count toward totals but cost a fraction of normal tokens, so they are included in the totals but cost far less. Output tokens are a tiny share of the total.

## What this covers

- **Agent harnessing & frameworks**: How I run many agents in parallel without losing control, and add every new module or adapter by shortest lossless integration.
- **System evolution: central orchestrator → clean-context agents**: How my GPU and batch systems got simpler and more reliable as the agents improved.
- **Velocity & scale**: Consistent, aggressive output over time, measured from my GitHub profile and local logs.

---

Part of my [GitHub profile](../README.md), chapter: System design.
