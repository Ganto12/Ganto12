# Pinned runtimes and baked images

One locked runtime per model mode, baked container images and GPU templates, and the container library behind them.

## Results

| Measured result |
|---|
| **LTX:** HighQ and distilled as separate runtimes |
| **MiniMax H3:** text-to-video, reference-to-video and first/last-frame as separate modes |
| **One pinned runtime per model mode:** GPT Image 2 via API as a hosted mode |
| **Box start:** 24 to 39 s with a cached image against 423 to 460 s uncached (measured) |
| **MLOps container library:** 11 Dockerfiles, ~10 image families |
| **MLOps container library:** 0 secrets in 129,503 files |

## How it works

### One pinned runtime per model mode

- LTX: two different models, two runtimes. The dev HighQ lane (its own Vast template and image) is slower and used where quality matters most; the distilled lane (its own isolated Docker runtime) is cheaper and faster and is the default for most work.
- MiniMax H3: text-to-video, reference-to-video and first/last-frame video run as separate modes inside the factory, each with its own locked workflow, settings and checks, chosen per shot.
- Benchmarks, my own GPUs against MiniMax's own H3 API ($0.80 per 10 s at 768P, priced per second of output for every task type), per 10 s of video: text-to-video at 768×1344 with native audio $0.030 (about $0.009 a 3 s clip): 27× cheaper; first/last-frame at 480×864 $0.012: 33× below MiniMax at 480P; reference-to-video, the cheapest full pipeline (generate at 480p, finish to 1080p with SeedVR2) $0.09–0.10: 8×; native 544×960 with no upscale $0.064: 12×; native 768×1344 with 3 image references $0.17: about 5×. Against Seedance 2 at 720p ($1.50–3.03 per 10 s) the gap is 15× to 100×.
- Cost split, reference-to-video: the 480p route is about $0.04–0.046 of H3 generation plus about $0.05–0.06 of SeedVR2 upscale on a 5090; the 544×960 route is $0.064 of generation plus $0.087 of upscale.
- Quality trade-off: the 480p-plus-upscale route is the cheapest; generating at a higher native resolution often looks better for cinematic shots, for about 1.7× the cost.
- Hosted APIs are modes too: GPT Image 2 through the OpenAI API (batch route, reference-led) sits beside the local runtimes and is chosen when it wins on cost or quality for that step.
- How a job picks its lane: drafts and volume go to the cheap, fast modes; hero shots and finals go to the high-quality modes; the factory records which mode made every output, so quality and cost stay comparable across modes.
- Locked the same way every time: pinned CUDA, PyTorch and model commits, a validated clean base, and a recorded runtime so any mode can be rebuilt or rolled back.
- How it transfers to cluster scale: each mode becomes its own container image and service, the scheduler places it on a GPU that fits its memory, and traffic is routed per job type. The Kubernetes GPU scheduling run in the simulation row shows the placement side.
- Trade-off: H3's modes are locked inside the factory rather than baked as separate images, for speed of iteration; baking them is a packaging step, not a redesign.

### Baked container images and GPU templates

- Approach: each factory has a pinned image (CUDA, PyTorch, commit and model files) and a Vast template, so a new GPU produces identical output and skips the 3 to 5 minute model download at launch (depending on the provider's network speed).
- Saving I found: Vast does not bill while an instance is loading its image, so pulling a big baked image costs nothing, while downloading models after start is paid GPU time.
- Measured: create-to-running was 24 to 39 s on hosts with the image cached and 423 to 460 s without; one 5090 with a 28 GB image layer took 8 min 16 s.
- Where it doesn't fit: MiniMax H3's model set (about 51.5 GB) could not be baked into one image, so it is fetched on start, about 470 s at about 111 MB/s.
- Quality lanes: LTX ran as two lanes, a 'dev HighQ' Vast template (image circuitdreams/ltx2) for top quality and a distilled lane with its own isolated Docker runtime for speed, picked per job. The worker defaults to an FP8 cast of the 22B model; an NVFP4 image was tried and failed at load.
- Rollback: each lane has a clean base snapshot (pinned CUDA 12.8, PyTorch 2.9.1 and a pinned LTX-2 commit) that is validated before it is saved, plus a recorded runtime export (ComfyUI and model commits, model inventory) to rebuild a clean image without snapshot bloat.
- Size-bounded variants: the MiniMax H3 image comes in three builds (full, without the text encoder, and without the text encoder and transformer), so the right one is chosen for each host's disk and the rest is fetched on start.

### MLOps container library

- Pinned CUDA/PyTorch/commit per image; template hash + config JSON = identical output on any new GPU.
- Per-mode runtime images: LTX HighQ and distilled each have their own pinned image (about 71 to 73 GB compressed), so a new GPU boots ready and any version can be rolled back.
- Baked images skip 3 to 5 minutes of model downloads per box, and Vast does not bill while an image loads.
- Secrets are never baked in: credentials are injected at run time, and a scan of 129,503 files found 0 secrets.

## What this covers

- **One pinned runtime per model mode (how it scales to clusters)**: Each model mode gets its own locked runtime, staggered by cost and quality and picked per job, the same pattern a cluster uses with one image per service.
- **Baked container images and GPU templates**: Models and dependencies baked into pinned Docker images and Vast templates, which amortises the 3 to 5 minute model download a new GPU would otherwise pay for.
- **MLOps container library**: Pinned, audited Docker images so every run is reproducible.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
