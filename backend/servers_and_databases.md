# Servers and databases: one hardened VPS for agents, queues and the database

The agents, the job queues and the NocoDB database share one smaller VPS, with writes hardened for parallel agents; a VPN-only workstation VM drives it, and every repo deploys on its own.

## Results

| Measured result |
|---|
| **Doing more on less:** 2 machines to 1 smaller one, same output |
| **Doing more on less:** repeated failures escalate to chat, then to me |
| **NocoDB-backed ops subsystems:** 4 subsystems |
| **NocoDB-backed ops subsystems:** AppOps has 12 tables |
| **Multi-machine production architecture:** 3 layers |
| **Multi-machine production architecture:** 13+ Mac repos, 8 VM repos, 10+ worktrees |

## How it works

### Doing more on less

- What moved: the agents, the queues and the database now share one smaller VPS. Jobs run through hardened queues in the background instead of competing for the machine at once.
- Database on the same box: writes are hardened so parallel agents don't corrupt or block each other, with automatic recovery when a write fails.
- Alerting ladder: a failure first tries to recover by itself; repeated failures with no recovery escalate to chat, then to me.
- Bottlenecks found and fixed: CPU exhaustion from too many agents at once; slow file-name crawling and directory scans (a metadata problem, not raw disk speed); and database contention from many agents writing in parallel. The fixes were bounded concurrency, queues and batched writes.
- Storage choices: local NVMe for hot work, mounted or object storage for bulk, chosen per job.

### NocoDB-backed ops subsystems

- SocialOps (demand), FactoryOps (execution), AppOps (apps, experiments, variants, onboarding, paywalls, analytics), Codex mission scoring.

### Multi-machine production architecture

- Tailscale/SSH sync, canonical sources, worktrees, deployment mirrors; every standalone repo deploys to Vercel on its own.

## Limits

- Honest limits: one operator and one main machine; the queue and alert design is what would carry over to a bigger fleet.

## What this covers

- **Doing more on less: VPS consolidation and hardening**: Moved the work of two Hetzner machines onto one smaller Contabo VPS without losing output, by hardening the queues and the database instead of buying hardware.
- **NocoDB-backed ops subsystems**: Databases that run the business side: demand, execution, apps and agent missions.
- **Multi-machine production architecture**: Mac plus VM production setup with standalone, independently deployable repos.

---

Part of my [GitHub profile](../README.md), chapter: Backend and infrastructure.
