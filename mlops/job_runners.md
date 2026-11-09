# Rented-GPU job runners: cheapest boxes, fully used, zero lost work

Parallel and idempotent job runners for ComfyUI and other workers on rented GPUs: cheap spot boxes, kept busy, no job lost or repeated.

## Results

| Measured result |
|---|
| **Parallel runner:** 3,551 lines, 4 isolated ComfyUI containers per GPU host, workers for 8 model families |
| Dec 2025 to Feb 2026 |
| orchestrator v1.12 to v4.1, 8 single-job scripts |
| 60+ worker scripts |
| 12+ orchestrators |

## Method (in brief)

- How it scales out: one host runs many lanes; across hosts, the race launchers rent several matching GPUs and keep the first proven one.
- Split into small scripts, each owning one job like an agent: an image sorter feeding Backblaze B2 object storage, the orchestrator on my workstation, a work scout that builds the job list, a pipeline worker syncing the job database, a Wan worker driving ComfyUI over SSH, a progress tracker, an uploader, and an error poller watching the ComfyUI queue.

## Limits

- Honest limits: an older system from the Wan era; it dispatches per host and per model family rather than through one central cross-host scheduler.

## What this covers

- **Rented-GPU job runners: cheapest boxes, fully used**: Rents the cheapest GPU that fits, checks it works, keeps it busy with queued video and image jobs, retries failures and uploads results, so no paid minute sits idle.
- **Cheap spot GPUs, zero lost work: an idempotent job runner**: Ran video jobs on cheap, interruptible spot GPUs instead of on-demand ones to cut cost. Every job is idempotent, so when a machine is taken away, a fresh one carries on: no lost work and no paid work repeated.
- **ComfyUI creative pipelines & orchestrator fleet**: A GPU worker fleet for image, video, music and SFX generation.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
