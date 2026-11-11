# vLLM speed-ups: FP8, FP8 KV cache and MTP, picked and validated

Built-in serving levers (FP8 weights, FP8 KV cache, multi-token prediction) picked and validated against a BF16 vLLM baseline, with the eval lab that measured them.

## Results

| Measured result |
|---|
| **Picked and validated built-in levers vs a BF16 vLLM baseline:** 3.08× single-stream, 1.86× at concurrency 32 (Max-Q 300 W, random 1k-in/512-out prompts, single run, median TPOT) |
| **vLLM speed-up brief:** $0.77 → $0.41 per 1M output tokens |
| **vLLM speed-up brief:** sparse prefill 1.44× at 64k (8/10 needle recall at 128k, full quality gate pending) |
| **Picked and validated FP8 weights, FP8 KV cache and MTP vs a BF16 vLLM baseline:** 3.08× single-stream, 1.86× at concurrency 32 (Max-Q 300 W, random 1k-in/512-out prompts, single run, median TPOT) |
| **Inference eval lab:** $0.77 → $0.41 per 1M output tokens |
| **Inference eval lab:** sparse prefill 1.44× at 64k (8/10 needle recall at 128k, full quality gate pending) |
| **Quantization and compression:** FP8 weights cut Qwen3.8-27B from 50.1 to 27.6 GiB |
| **Quantization and compression:** with FP8 KV cache decode went 1.85× single-stream, 3.08× with MTP (multi-token prediction, a form of speculative decoding) (Max-Q 300 W, random 1k-in/512-out prompts, single run) |

## Setup and how it was measured

- Setup: one NVIDIA RTX PRO 6000 Blackwell Max-Q (96 GB, 300 W power limit), vLLM 0.32.0, torch 2.13.0, CUDA 13.0; model Qwen3.8-27B, a hybrid of 48 Gated DeltaNet layers and 16 full-attention layers.
- Bench: vllm bench serve with random prompts, 1,024 tokens in and 512 out. Single-stream is 4 requests sent one at a time; concurrency 32 is 64 requests with 32 in flight. One run per config. Throughput is output tokens a second; per-stream decode speed comes from the median time per output token (TPOT).

## How it works

### vLLM speed-up brief

- Levers, all built into vLLM 0.32.0: the official block-wise FP8 weights, an FP8 KV cache and MTP speculative decoding (3 draft tokens). On top, a third-party sparse prefill kernel (FlashPrefill V1) patched into the 16 full-attention layers. The work was picking these for this model and validating each one, not inventing them.
- Results vs the BF16 baseline (BF16 weights + CUDA graphs, vLLM's default): 3.08× single-stream (25.4 → 78.2 tok/s) and 1.86× at concurrency 32 (495.7 → 921.3 tok/s) (Max-Q 300 W, random 1k-in/512-out prompts, single run, median TPOT). Cost per 1M output tokens $0.77 → $0.41 follows from that. Sparse prefill: 1.44× end to end on a cold 64k prefill (13.99 → 9.74 s), 8/10 needle recall at 128k; full quality gate pending.
- Illustrative, not measured on B300: carried to a 192-B300 fleet, 1.86× would be about +165 GPUs of capacity, roughly $13.5m a year of tokens to sell (see What a speed-up is worth).
- Versus market prices (output tokens, per 1M): OpenRouter lists Qwen3.8-27B at about $2.55–3.20 (typically $3.00), and the cheapest direct listing found was $0.90. My $0.41 is about 7× below the typical OpenRouter price and about 2× below the cheapest listing. It is a marginal cost at full batch-32 load on a rented card (implied box cost about $1.36 an hour), output tokens only, before margin, idle time and SLAs. At 46% utilisation it matches the $0.90 listing; at 14% it matches $3.00; at 30% it is about $1.37. Prices are from third-party trackers, snapshot August–September 2026.

### Inference eval lab

- Qwen3.8-27B on one RTX PRO 6000 Blackwell Max-Q (300 W) with vLLM 0.32.0: FP8 weights, FP8 KV cache and MTP speculative decoding, all built into vLLM, each picked and validated against a BF16 baseline.
- Measured prefix caching: the hit rate tracks the workload's theoretical maximum (95.6% vs 96.5% in the steady-state agent replay); found what breaks it (a timestamp at the top of the prompt drops it to 0%).
- Long agent session (250k tokens): prefix-cache hit rate 96.5% at 128–192k and 97.5% at 192–260k; two compactions at about 237k (71 s and 99 s); 10 of 10 planted facts kept every time.
- Step-graph gap map: about 6 ms of a 29 ms MTP step (about 20%) is sized for two serving-path fixes, a cuBLAS tile mis-pick and a 3x lm_head read (see the physics-gap write-up).

### Quantization and compression

- Weights and memory: the official block-wise FP8 (e4m3) Qwen3.8-27B is 27.55 GiB against 50.13 GiB in BF16. An FP8 KV cache roughly doubles the KV pool, 1,041,043 tokens against 516,600.
- Speed on one RTX PRO 6000 Max-Q (300 W) with vLLM 0.32, vllm bench serve, random 1,024-in/512-out prompts, single run: BF16 25.4 tokens a second at one request; FP8 weights plus FP8 KV 46.9 (1.85×); adding MTP ×3 78.2 (3.08×). At 32 concurrent requests, 495.7 to 921.3 (1.86×). Cost per million output tokens $0.77 to $0.41.
- Quality: MTP speculative decoding keeps the target model's output distribution by design.
- What did not help, measured: FP8 KV gives +2% decode at 8k context and +12% at 250k. A Triton FP8 attention kernel was 0.76× at 64k and 0.66× at 128k. Custom kernels on top of FP8 plus MTP were 3% slower. Sage attention with compile was 0.98× on a 5090.
- ComfyUI fleet: 383 of 473 workflow files reference quantized weights (a count of file references, not proof of execution): Wan 2.2 and 2.1 fp8, Qwen-Image-Edit fp8, SCAIL and VACE fp8, FLUX fp8, LTX-2 fp8, SeedVR2 fp8, plus Wan and FLUX GGUF variants in the templates.
- Production settings: Wan 2.2 14B image-to-video on Kijai fp8_e4m3fn_scaled weights with sage attention, torch.compile and block swap; LTX-2.3 22B with runtime fp8-cast as the locked default (all 9 recorded fp8-cast worker reports ok); SeedVR2 7B fp8-mixed upscaling a 121-frame clip to 1080p in 165 seconds.
- Visual quality cost (video): speed levers are judged by how much they change the output, not just by speed. Two baseline runs were bit-identical, which gives a zero noise floor; SageAttention FP8 cut 15 to 27% of the time per step but changed the output, so it stays out of the canonical engine, while FP8 presets run at several step counts in 480p and 768p tiers.
- Staggered runtimes by need: cheaper, faster lanes (distilled models, FP8, lower tiers) for drafts and volume; slower high-quality lanes (HighQ models, higher tiers, SeedVR2 upscale) for finals. Each job is matched to the lane whose cost and quality fit it (see One pinned runtime per model mode).
- Runtime quantization in my own code: Kyutai TTS 1.6B quantized to 8-bit with MLX at load; NV-Embed-v2 in 8-bit as a small-GPU fallback; QLoRA fine-tunes on 4-bit bases.
- Debugging a quantized stack: a rented host produced all-black Wan output. I isolated it by switching quantization off (BF16), swapping attention backends and the fp8 fast path, and traced it to the runtime stack, not the weights.
- Kernels: LeetGPU accepted NVFP4 matmul, FP8 block-scaled GEMM, a KV-cache attention with quantized cache and fused RMSNorm plus W8A8 linear. These were correctness-only on a T4, which has no FP8 or FP4 hardware.

## Limits

- Honest limit: I have not made my own quantized checkpoint. No calibration, AWQ, GPTQ or NVFP4 export. I used published checkpoints and runtime casts, and an LTX-2.3 NVFP4 attempt failed at load. Closing that gap is the obvious next piece of work.

## What this covers

- **vLLM speed-up brief: built-in levers, picked and validated**: Which built-in vLLM levers I picked for a 27B hybrid model, how I validated them against a BF16 baseline, and exactly how it was measured.
- **Inference eval lab (vLLM)**: Served a 27B model on one GPU and measured every speed lever against predictions written beforehand.
- **Quantization and compression: deploy, validate, measure**: Running quantized models in production and measuring what each format costs in speed, memory and, for video, visual quality.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
