# Video inference on my own GPUs: MiniMax H3 at 27× less than its own API

Self-hosted video generation (MiniMax H3 text-to-video, reference-to-video and first/last-frame), each mode a locked runtime, priced per second of video against the model maker's API.

## Results

| Measured result |
|---|
| **MiniMax H3 video factory on my own GPUs:** Text-to-video 27× below MiniMax's own H3 API per second of video |
| **MiniMax H3 video factory on my own GPUs:** reference-to-video 5×–12× below |
| **MiniMax H3 video factory on my own GPUs:** an 11,764-clip batch run |
| **Per 10 s of video, MiniMax H3 on my own GPUs vs MiniMax's own H3 API:** text-to-video 27×, first/last-frame 33×, reference-to-video 5×–12× |
| **Local vs API cost arbitrage:** 15×–100× below Seedance 2 |
| **images:** about $2–2.50 per 1,000 vs about $250 per 1,000 at default high quality |
| **Self-hosted inference stack:** Video generation 27× below MiniMax's own H3 API on text-to-video, 50×–100× below Seedance 2 on text-to-video |

## How it works

### MiniMax H3 video factory on my own GPUs

- Modes: text-to-video, reference-to-video and first/last-frame video run as separate modes, each with its own locked workflow, settings and checks, chosen per shot.
- Benchmarks, my own GPUs against MiniMax's own H3 API ($0.80 per 10 s at 768P, priced per second of output for every task type), per 10 s of video: text-to-video at 768×1344 with native audio $0.030 (about $0.009 a 3 s clip): 27× cheaper; first/last-frame at 480×864 $0.012: 33× below MiniMax at 480P; reference-to-video, the cheapest full pipeline (generate at 480p, finish to 1080p with SeedVR2) $0.09–0.10: 8×; native 544×960 with no upscale $0.064: 12×; native 768×1344 with 3 image references $0.17: about 5×. Against Seedance 2 at 720p ($1.50–3.03 per 10 s) the gap is 15× to 100×.
- Cost split, reference-to-video: the 480p route is about $0.04–0.046 of H3 generation plus about $0.05–0.06 of SeedVR2 upscale on a 5090; the 544×960 route is $0.064 of generation plus $0.087 of upscale.
- Quality trade-off: the 480p-plus-upscale route is the cheapest; generating at a higher native resolution often looks better for cinematic shots, for about 1.7× the cost.
- LTX 2.3 on the same rented GPUs came in at about $0.006 a clip against Lightricks' own API at $0.03–0.04 a second at 720p (receipts on the archived VM).
- Scale: an 11,764-clip batch ran on RunPod serverless; Vast race launchers rent the cheapest matching GPUs and keep the first that proves ready.
- Packaging: the model set is about 51.5 GB, too large for one baked image, so it is fetched on start (about 470 s at about 111 MB/s). The image comes in three builds (full, without the text encoder, and without the text encoder and transformer), chosen per host disk.
- Speed-up A/B (one RTX PRO 6000, 576p, 4 s, 96 frames, 20 steps, fixed seed): two baseline runs were bit-identical, a clean zero noise floor. Profiling one 5.2 s step put attention at about 55%. torch.compile and CUDA graphs gave no safe gain; SageAttention FP8 cut 15–27% per step but changes the output, so it is kept out of the canonical engine.
- Watching output: a gallery refreshes from the VM and object storage without downloading MP4s.

### Local vs API cost arbitrage

- Motion video: local SCAIL vs Kling Motion Control 3 → ~17.5× cheaper.
- Arbitrage inside the GPU market too: Vast was far cheaper at first, so most factories ran there; when GPU prices started rising and the gap narrowed, work moved to Verda where it fit better. Inside Vast, the broken slot machine was baked-image runtimes: a pinned image with the whole runtime inside cuts slow starts and broken boots, which is where the real cost of cheap hosts hides.
- Benchmarks, my own GPUs against MiniMax's own H3 API ($0.80 per 10 s at 768P, priced per second of output for every task type), per 10 s of video: text-to-video at 768×1344 with native audio $0.030 (about $0.009 a 3 s clip): 27× cheaper; first/last-frame at 480×864 $0.012: 33× below MiniMax at 480P; reference-to-video, the cheapest full pipeline (generate at 480p, finish to 1080p with SeedVR2) $0.09–0.10: 8×; native 544×960 with no upscale $0.064: 12×; native 768×1344 with 3 image references $0.17: about 5×. Against Seedance 2 at 720p ($1.50–3.03 per 10 s) the gap is 15× to 100×.
- Cost split, reference-to-video: the 480p route is about $0.04–0.046 of H3 generation plus about $0.05–0.06 of SeedVR2 upscale on a 5090; the 544×960 route is $0.064 of generation plus $0.087 of upscale.
- Quality trade-off: the 480p-plus-upscale route is the cheapest; generating at a higher native resolution often looks better for cinematic shots, for about 1.7× the cost.
- LTX 2.3 on the same rented GPUs came in at about $0.006 a clip against Lightricks' own API at $0.03–0.04 a second at 720p (receipts on the archived VM).
- Images: GPT Image 2 batch pipeline → 57.5×–125× cheaper than standard routes.
- Rule: if an API costs under 2× local, keep it local; otherwise A/B the API against local plus a judge pass. Next frontier: Krea 2 route toward 500×.
- Images: GPT Image 2 at its default high-quality setting costs about $250 per 1,000 images even on the batch route. My route pulls high quality out of the cheapest tier using reference images, image compression and batching, at about $2 to $2.50 per 1,000.

### Self-hosted inference stack

- vLLM self-hosting for LLM workloads; ComfyUI fleets for Wan 2.2, LTX-2.3, Seedance, SCAIL.
- Origin: Midjourney video capped me at about 24 videos a day, and only while sitting at the computer, so I moved to Wan 2.1, then Wan 2.2, on rented GPUs.
- About 10 Docker GPU image families; 11 verified Dockerfiles; Vast/RunPod launchers.
- Each factory step can run on a local model or an API, with a clear default and a clean swap.

## Limits

- Honest limit: list API prices change (MiniMax pay-as-you-go rates read 7 November 2026), and my side is warm GPU rental only, excluding startup, idle, storage, transfer and review.

## What this covers

- **MiniMax H3 video factory on my own GPUs**: Text-to-video, reference-to-video and first/last-frame video generation on rented GPUs, each mode a locked runtime picked per shot, at a fraction of the model maker's own API price.
- **Local vs API cost arbitrage**: Pick, per step, the cheapest model that meets quality.
- **Self-hosted inference stack**: Ran open models on my own GPUs early, with vLLM before SGLang was common.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
