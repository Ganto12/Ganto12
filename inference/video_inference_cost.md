# Video inference on my own GPUs: MiniMax H3 at 27× less than its own API

Self-hosted video generation (MiniMax H3 text-to-video, reference-to-video and first/last-frame), each mode a locked runtime, priced per second of video against the model maker's API.

## Results

| Measured result |
|---|
| Text-to-video 27× below MiniMax's own H3 API per second of video |
| reference-to-video 5×–12× below |
| an 11,764-clip batch run |
| **Per 10 s of video, MiniMax H3 on my own GPUs vs MiniMax's own H3 API:** text-to-video 27×, first/last-frame 33×, reference-to-video 5×–12× |
| 15×–100× below Seedance 2 |
| **images:** about $2–2.50 per 1,000 vs about $250 per 1,000 at default high quality |
| Video generation 27× below MiniMax's own H3 API on text-to-video, 50×–100× below Seedance 2 on text-to-video |

## Method (in brief)

- Modes: text-to-video, reference-to-video and first/last-frame video run as separate modes, each with its own locked workflow, settings and checks, chosen per shot.
- Packaging: the model set is about 51.5 GB, too large for one baked image, so it is fetched on start (about 470 s at about 111 MB/s).

## Limits

- Honest limit: list API prices change (MiniMax pay-as-you-go rates read 7 November 2026), and my side is warm GPU rental only, excluding startup, idle, storage, transfer and review.

## What this covers

- **MiniMax H3 video factory on my own GPUs**: Text-to-video, reference-to-video and first/last-frame video generation on rented GPUs, each mode a locked runtime picked per shot, at a fraction of the model maker's own API price.
- **Local vs API cost arbitrage**: Pick, per step, the cheapest model that meets quality.
- **Self-hosted inference stack**: Ran open models on my own GPUs early, with vLLM before SGLang was common.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
