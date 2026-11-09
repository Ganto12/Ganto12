# Object storage: GPU workers write straight to B2

Backblaze B2, used through its S3-compatible interface, as the shared storage for everything the factories make, so a GPU box can be destroyed the moment its job is done.

## Results

| Measured result |
|---|
| Vast GPU workers write straight to B2 |
| the workstation VM is backed up to it |

## How it works

- Vast workers write their outputs directly to B2, so a GPU box can be destroyed the moment its job is done.
- The workstation VM is backed up to B2.
- A dedicated, fail-closed credential for Vast workers: the route refuses to start a paid box if the wrong key, bucket or a default config is selected.
- Verified uploads: outputs are listed back from B2 before a job is marked uploaded, which is what made spot GPUs safe to use.

---

Part of my [GitHub profile](../README.md), chapter: Backend and infrastructure.
