# Self-auditing agents: one report contract and a weekly autopsy

Every agent reports in one contract, and a weekly autopsy and debate pipeline audits and patches the agents' own rules.

## Results

| Measured result |
|---|
| **Agent reporting standard:** Deployed on Claude Code + Codex, 2 machines, weekly self-audit |
| **Weekly autopsy & agent debate factory:** 4-stage pipeline |

## How it works

### Agent reporting standard

- WHAT CHANGED / WHY / WHAT NEXT; bad news has a mandatory place.
- One source-of-truth file per workstream; options as Quick vs Optimal with time and tradeoff.
- Weekly transcript autopsy patches the rules; switchable report modes deployed to every machine.
- Adapted from a Twitch founder's status-update discipline.

### Weekly autopsy & agent debate factory

- Extract → sanitise → audit for failure patterns → render patches and judge checks.
- Subagent debate factory: two specialist agents argue for 10 turns to improve hooks and copy.

## What this covers

- **Agent reporting standard**: One contract for every AI agent I run, so I supervise dozens from final messages alone.
- **Weekly autopsy & agent debate factory**: The agent system audits itself weekly and patches its own rules.

---

Part of my [GitHub profile](../README.md), chapter: System design.
