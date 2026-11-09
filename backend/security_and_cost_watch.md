# Security and cost watch: VPN-only machines, watchers on every bill

Hardened machines with no public services, secrets kept out of code, and automated watchers that flag agent mistakes, boxes left running and billing growth.

## Results

| Measured result |
|---|
| Hardened, VPN-only workstation |
| automated watchers alert on chat for agent mistakes and runaway billing |

## How it works

- Workstation: development runs on a Contabo VM reached only from my workstation over Tailscale; an earlier Hetzner VM did the same job and was retired with a snapshot kept. The Mac is a thin client.
- Security: hardened, locked-down machines with no public services, access over Tailscale only, and secrets kept out of code (0 secrets found across 129,503 files in the container library scan).
- GPUs: rented by the hour on Vast (most factories), Verda (vLLM labelling and multi-GPU training) and RunPod, chosen by price and fit per job.
- Watchers: automated checks post to chat when something goes wrong: an agent breaking a run, boxes left running, or billing growing on Hetzner, Vast or B2. A daily infra ledger with an allowlist backs this up, with a watchdog if a scan is missed.
- Agents cannot starve the server: a systemd limit on the agent user caps memory at 38 GB soft and 42 GB hard, 4 GB of swap and 8,192 tasks on a 47 GB box, so a runaway coding agent cannot take the RAM that the database and SSH need.

## Limits

- Honest limits: small-team ops on a handful of machines; the workstation disk runs close to full and needs watching.

---

Part of my [GitHub profile](../README.md), chapter: Backend and infrastructure.
