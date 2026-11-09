# Movie factory: a film as a gated graph

A long-form film pipeline built as a fixed graph of stages, each behind a quality gate, with every choice recorded as data.

## Results

| Measured result |
|---|
| 34-node graph with 7 loops, 193 gates, 42 quality dimensions |
| passed four clean-context audits |

## How it works

- Structure: a fixed graph of 34 nodes and 7 loops, read as three blocks, with a gate at every step. Gates, axes, routes and banned phrases live in data files, so the validator can run every gate against known-bad and known-good test cases.
- Modular axes and harnesses: the 42 quality dimensions live in data files beside the gates and routes, so a new film is a new set of axis values rather than new code. The validator, the stage seal and the green-light ladder are the harness that proves each stage before the next one runs.
- The 42 quality dimensions, in 8 groups: story and scope (story, versions, canon locks, open decisions, dialogue); canon assets (characters and wardrobe, sets and props, look, reference grammar, banned phrases); camera and frame (frame shape, stills, motion, storyboard); risk and budget (drift risk, A/B roll, render tier, video preset, tries budget); audio and upscale (reference audit, dialogue, sound design, music, upscale); route, cost and delivery (route, cost, storage, SHA-chain invalidation); review and QA (review states, prop placement, re-spin, drift QA, reference registry, redo loops); behaviour and image generation.
- Quality process: the first spec failed an independent clean-context audit with 11 serious findings. Re-audits found 5, then 4, then 4 more, each fixed and traced, until the validator checked the gates by running them, not by matching strings.
- One source of truth: the film factory holds no copy of route decisions or measured numbers. It links to the video test bench for routes and benchmarks, and where a line disagrees with those files, they win.
- Video route (from the test bench): FP8 presets at several step counts, 480p and 768p tiers, voices swapped in post, and SeedVR2 8-bit upscale on a large-memory card. Whole-film pipelines were costed in FP8 against BF16, tiled against untiled and across two GPU cards. See the quantization row.
- Code: stage modules for planning frames, image generation, GPU registration, a stage seal, a green-light ladder, a prep gate and an upscale driver, each with tests. The gates are described under Automated gates.
- Speed-up A/B on the video engine (MiniMax H3, ComfyUI, one RTX PRO 6000, 576p, 4 s, 96 frames, 20 steps, fixed seed): two baseline runs were bit-identical (max pixel difference 0), giving a clean zero noise floor. Each arm changed only the model link into the sampler and was scored frame by frame on PSNR.
- Profiler trace of one 5.2 s step: attention (FlashAttention-2) 2.85 s, about 55%; FP8 scaled matmuls 1.18 s; one BF16 matmul per block 0.54 s; elementwise about 0.6 s. Baseline 108.6 to 109.3 s per clip, peak 51.6 GB.
- What did not help, measured: torch.compile gave no per-step gain and cost about 10 s to compile; compile with CUDA graphs failed twice (allocator and capture errors); forced cuDNN attention was 1% faster, inside run spread, and only after overriding ComfyUI's backend list (the first try was a silent no-op); cuDNN plus CUDA graphs was slower at 114.3 s.
- Verdict: no safe speed-up on this stack. The one large lever, SageAttention FP8 (15 to 27% less time per step in earlier runs), is kept out of the canonical engine because it changes the output. The whole test cost about 2.60 USD of GPU time.
- Factory lineage: the third ask of the same thesis, after social posts (PostQueue Prep) and long-form video (Ready Longs); it moves the axes and gates into data files (see Factory lineage).

---

Part of my [GitHub profile](../README.md), chapter: Content engine.
