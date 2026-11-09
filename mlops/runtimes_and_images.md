# Pinned runtimes and baked images

One locked runtime per model mode, baked container images and GPU templates, and the container library behind them.

## Results

| Measured result |
|---|
| **LTX:** HighQ and distilled as separate runtimes |
| **MiniMax H3:** text-to-video, reference-to-video and first/last-frame as separate modes |
| GPT Image 2 via API as a hosted mode |
| **Box start:** 24 to 39 s with a cached image against 423 to 460 s uncached (measured) |
| 11 Dockerfiles, ~10 image families |
| 0 secrets in 129,503 files |

## Method (in brief)

- How a job picks its lane: drafts and volume go to the cheap, fast modes; hero shots and finals go to the high-quality modes; the factory records which mode made every output, so quality and cost stay comparable across modes.
- How it transfers to cluster scale: each mode becomes its own container image and service, the scheduler places it on a GPU that fits its memory, and traffic is routed per job type.
- Approach: each factory has a pinned image (CUDA, PyTorch, commit and model files) and a Vast template, so a new GPU produces identical output and skips the 3 to 5 minute model download at launch (depending on the provider's network speed).

## What this covers

- **One pinned runtime per model mode (how it scales to clusters)**: Each model mode gets its own locked runtime, staggered by cost and quality and picked per job, the same pattern a cluster uses with one image per service.
- **Baked container images and GPU templates**: Models and dependencies baked into pinned Docker images and Vast templates, which amortises the 3 to 5 minute model download a new GPU would otherwise pay for.
- **MLOps container library**: Pinned, audited Docker images so every run is reproducible.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
