# Infrastructure and security: workstation, VM and GPU clouds

The multi-machine production setup: a VPN-only workstation, a control VM, five GPU clouds over SSH, object storage, and consolidation onto less hardware.

## Results

| Measured result |
|---|
| 3 layers |
| 13+ Mac repos, 8 VM repos, 10+ worktrees |
| Vast, Verda, RunPod, Hetzner and Contabo boxes run over SSH |
| Hardened, VPN-only workstation |
| automated watchers alert on chat for agent mistakes and runaway billing |
| 2 machines to 1 smaller one, same output |
| repeated failures escalate to chat, then to me |
| Vast GPU workers write straight to B2 |
| the workstation VM is backed up to it |

## Limits

- Honest limits: small-team ops on a handful of machines; the workstation disk runs close to full and needs watching.
- Honest limits: one operator and one main machine; the queue and alert design is what would carry over to a bigger fleet.

## What this covers

- **Multi-machine production architecture**: Mac plus VM production setup with standalone, independently deployable repos.
- **Linux and remote GPU operations**: Everything runs over SSH on rented Linux boxes I set up, pin, watch and tear down myself.
- **Cloud ops and security: Hetzner, Contabo, Vast, Verda, RunPod**: A rented VM is my main workstation, with GPUs rented by the hour and storage on B2, all watched for cost every day.
- **Doing more on less: VPS consolidation and hardening**: Moved the work of two Hetzner machines onto one smaller Contabo VPS without losing output, by hardening the queues and the database instead of buying hardware.
- **B2 object storage (S3-compatible)**: Backblaze B2, used through its S3-compatible interface, as the shared storage for everything the factories make.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
