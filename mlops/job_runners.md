# Rented-GPU job runners: cheapest boxes, fully used, zero lost work

Parallel and idempotent job runners for ComfyUI and other workers on rented GPUs: cheap spot boxes, kept busy, no job lost or repeated.

## Results

| Measured result |
|---|
| **Parallel runner:** 3,551 lines, 4 isolated ComfyUI containers per GPU host, workers for 8 model families |
| **Cheap spot GPUs, zero lost work:** Dec 2025 to Feb 2026 |
| **Cheap spot GPUs, zero lost work:** orchestrator v1.12 to v4.1, 8 single-job scripts |
| **ComfyUI creative pipelines &…:** 60+ worker scripts |
| **ComfyUI creative pipelines &…:** 12+ orchestrators |

## How it works

### Rented-GPU job runners

- Era note: built in the Wan era, before LTX and far more capable models arrived; the production path has since moved on. Kept here for the architecture decisions and the ops skills it shows, not as the current stack.
- GPU saturation by design: the model stays loaded on a warm box, jobs are queued ahead so a free lane pulls its next one at once, inputs are staged in rolling verified chunks before they are needed, outputs upload in the background, and several lanes run in parallel per host in tmux or one container per GPU. Not yet measured as a GPU busy-percentage timeline.
- Vast runner (v4.1.2): rents and verifies each instance with a set-up check loop before any job runs, locks to EU hosts for latency, runs periodic monitors per worker, and shows setup, workers, uploader and pipeline state in a live terminal dashboard.
- Parallel runner on Verda: several isolated ComfyUI containers on one GPU host (one port each), so different jobs run side by side without sharing state.
- Workers per model family: Qwen image edit, LTX, SeedVR image and video upscaling, Wan, Wan GP, SCAIL and TTS, each with its own settings (for example torch.compile mode and attention backend for SeedVR).
- Fail-safes: Docker pull retries with backoff, per-job retries with backoff, job and start-up timeouts, verified input staging from B2 in rolling chunks, and results written back to storage.
- How it scales out: one host runs many lanes; across hosts, the race launchers rent several matching GPUs and keep the first proven one.
- Exploration (Feb to Apr 2026, before I knew GPUs well): Parallel lanes on Verda: one Docker container per lane, each pinned to its own GPU or MIG slice with `--gpus device=`, on its own port. MIG layouts are built automatically and a lane is refused if its slice is under the minimum memory. A per-process VRAM cap guards shared cards, and one shared queue feeds whichever lane frees up first. MIG auto-layout was added on 3 Mar 2026. Underneath, Docker GPU passthrough via the NVIDIA Container Toolkit, set up on each host and checked by a pre-flight before any job runs.
- Exploration (9 Mar 2026): Multi-GPU sharder for LTX: splits a job list round-robin across the chosen GPUs, starts one pinned worker per GPU, polls them every 5 seconds and can stop the rest if one fails. Static shards, no work stealing.

### Cheap spot GPUs, zero lost work

- Built from December 2025 (first commit 9 Dec 2025) for Wan-era video generation, when models were far weaker and GPU time was the main cost.
- Split into small scripts, each owning one job like an agent: an image sorter feeding Backblaze B2 object storage, the orchestrator on my workstation, a work scout that builds the job list, a pipeline worker syncing the job database, a Wan worker driving ComfyUI over SSH, a progress tracker, an uploader, and an error poller watching the ComfyUI queue.
- Idempotent from v1.2: the work scout checks the job database for anything already marked done or uploaded and skips it. A Vast box could be killed or outbid mid-batch, and a fresh box carried on with only the unfinished work.
- Verified uploads: the uploader copies the run's output folder to a clean target in B2, lists B2 to confirm what actually landed, and only then moves each job from done to uploaded. A file still being written can never be marked uploaded by mistake.
- Small disks, because outputs leave the box as the batch runs; the database, not the GPU box, holds the truth about what is finished.
- Iterated hard: orchestrator v1.12 to v4.1, worker to v1.32, uploader to v2.14, scout to v4.6. Milestones: v1.2 skip-done idempotency; v1.22 retry flow; v1.23.27 dedup by filename across all runs; v2.0 and v2.1 live dashboard and reuse of a warm box without redoing set-up; v3.0 and v3.1 unified set-up and one shared database config, which fixed scripts writing to the wrong base.
- Guarantees, stated honestly: the orchestrator polls rather than receiving worker heartbeats; there are no job leases, so recovery works by re-scanning what is done; scheduling is first free worker. The guarantee it does give is effectively-once results through skip-done plus verified uploads.
- Rough savings, estimated, not measured: interruptible Vast GPUs usually run well below on-demand prices, and the skip-done design is what made them safe to use. Small disks cut storage billing and widen the pool of hosts that qualify.
- Since retired: the production path moved to LTX and newer factories. It stays here for the architecture and ops decisions it shows.

### ComfyUI creative pipelines &…

- Wan 2.2 (4 variants), LTX-2.3 (4 distillation modes, 3 GPU classes), Kling, SCAIL, Seedance, SeedVR2 upscale, Hunyuan SFX, AceStep music, TangoFlux.
- Race-and-keep, multi-GPU sharding and GPU pooling; MiniMax H3 batch video of 11,764 clips on RunPod serverless.

## Limits

- Honest limits: an older system from the Wan era; it dispatches per host and per model family rather than through one central cross-host scheduler.

## What this covers

- **Rented-GPU job runners: cheapest boxes, fully used**: Rents the cheapest GPU that fits, checks it works, keeps it busy with queued video and image jobs, retries failures and uploads results, so no paid minute sits idle.
- **Cheap spot GPUs, zero lost work: an idempotent job runner**: Ran video jobs on cheap, interruptible spot GPUs instead of on-demand ones to cut cost. Every job is idempotent, so when a machine is taken away, a fresh one carries on: no lost work and no paid work repeated.
- **ComfyUI creative pipelines & orchestrator fleet**: A GPU worker fleet for image, video, music and SFX generation.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
