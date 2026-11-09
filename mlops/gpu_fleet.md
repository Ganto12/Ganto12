# GPU fleet: buying agents that race the cheapest boxes, run over SSH

Agents that rent the cheapest GPU boxes in parallel, keep the first that proves it works and shut the rest down; overseers that keep spend low; and every box driven over SSH from one control VM.

## Results

| Measured result |
|---|
| **GPU buying agents:** Up to 10 live candidates per wave, 3 waves, first-proven instance wins |
| **Vast.ai fleet:** 12+ launchers, 6 skill-level overseers |
| **Linux and remote GPU operations:** Vast, Verda, RunPod, Hetzner and Contabo boxes run over SSH |

## How it works

### GPU buying agents

- Race launchers per model (LTX HighQ, SeedVR2 image and video upscale, Kyutai TTS, ACE-Step music, vLLM Omni audio QA): rent 5–10 cheapest matching instances from a pinned template and keep only the first that proves real readiness (a real SSH command plus finished bootstrap, trusting actual status over flaky SSH). The rest are destroyed.
- Overseers delegate each wave to clean-context subagents, wait about 30 minutes for model cache and bootstrap, retry up to 3 ten-instance waves, and cap live candidates at 10 so spend stays bounded.
- Hygiene: a stuck-instance cleaner destroys instances hung in loading states; a daily infra ledger reports Hetzner, Vast and B2 spend growth to chat.
- Watching output: live remote galleries stream LTX clips and Seed/Qwen images as they render, and a MiniMax H3 gallery refreshes from VM and B2 without downloading MP4s.
- Underneath: priority-based GPU hunting across Vast, Verda and Paperspace; a central Vast lifecycle manager; pinned Docker images; MiniMax H3 batch video of 11,764 clips on RunPod serverless.

### Vast.ai fleet

- Wan, ComfyUI, Kyutai STT/TTS, Hunyuan SFX, SeedVR2, Qwen image-edit, LTX; central lifecycle manager.
- Race launchers rent 5 to 10 matching boxes, keep the first that passes a real readiness check, and destroy the rest.
- Overseers watch running jobs, retry failures, and tear boxes down when work ends, so nothing bills idle; chat watchers flag runaway spend.

### Linux and remote GPU operations

- Remote control: every GPU box is driven over SSH from a central manager. Setup is checked by a real SSH command before any job runs, and long jobs run in tmux sessions on the box (the LTX worker restarts a dead session up to 2 times) so a dropped connection never kills a job.
- GPU passthrough into Docker on Verda (from Jan 2026): a host set-up script installs the NVIDIA Container Toolkit from NVIDIA's signed repo and wires it into Docker with nvidia-ctk. A worker pre-flight then fails the run if containers cannot see the GPU. On top of that: MIG switched on per GPU with nvidia-smi, one container per lane pinned with --gpus device= to its own GPU or MIG slice, a lane refused if its slice is under the memory floor, and a per-process VRAM cap on shared cards. This is container GPU passthrough, not VM PCI passthrough.
- Resource control on the control VM: a systemd slice limit on the agent user (38 GB soft and 42 GB hard memory, 4 GB swap, 8,192 tasks on a 47 GB box) so agents cannot starve the database or SSH. systemd timers guard shared memory and temp-folder growth and prune the database audit log.
- Locked-down access: the control VM is reachable only over Tailscale, with local services bound to 127.0.0.1. chat watchers flag agent mistakes and runaway billing.
- Storage plumbing: rclone to B2 with a dedicated credential for Vast workers. The route fails closed before a paid box is rented if the wrong key, bucket or a default config is selected, and credentials are injected at runtime, never baked into images.
- Bring-up and teardown: race launchers rent several matching boxes, keep the first that passes a real readiness check, and destroy the losers, so I pay only for the box that works.

## What this covers

- **GPU buying agents: race the cheapest boxes, keep the first that works**: Agents rent 5 to 10 of the cheapest matching GPUs, keep the first one that proves it works, shut the rest down, and clean up anything left running, so spend stays bounded.
- **Vast.ai fleet: launchers that keep GPU spend low**: Launchers and overseers that bring GPU boxes up at the cheapest rate, run the work, and tear them down the moment it ends, so nothing bills idle.
- **Linux and remote GPU operations**: Everything runs over SSH on rented Linux boxes I set up, pin, watch and tear down myself.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
