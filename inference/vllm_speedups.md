# vLLM speed-ups: FP8, FP8 KV cache and MTP, picked and validated

Built-in serving levers (FP8 weights, FP8 KV cache, multi-token prediction) picked and validated against a BF16 vLLM baseline, with the eval lab that measured them.

## Results

| Measured result |
|---|
| **Picked and validated built-in levers vs a BF16 vLLM baseline:** 3.08× single-stream, 1.86× at concurrency 32 (Max-Q 300 W, random 1k-in/512-out prompts, single run, median TPOT) |
| $0.77 → $0.41 per 1M output tokens |
| sparse prefill 1.44× at 64k (8/10 needle recall at 128k, full quality gate pending) |
| **Picked and validated FP8 weights, FP8 KV cache and MTP vs a BF16 vLLM baseline:** 3.08× single-stream, 1.86× at concurrency 32 (Max-Q 300 W, random 1k-in/512-out prompts, single run, median TPOT) |
| FP8 weights cut Qwen3.8-27B from 50.1 to 27.6 GiB |
| with FP8 KV cache decode went 1.85× single-stream, 3.08× with MTP (multi-token prediction, a form of speculative decoding) (Max-Q 300 W, random 1k-in/512-out prompts, single run) |

## Setup and how it was measured

- Setup: one NVIDIA RTX PRO 6000 Blackwell Max-Q (96 GB, 300 W power limit), vLLM 0.32.0, torch 2.13.0, CUDA 13.0; model Qwen3.8-27B, a hybrid of 48 Gated DeltaNet layers and 16 full-attention layers.
- Bench: vllm bench serve with random prompts, 1,024 tokens in and 512 out. Single-stream is 4 requests sent one at a time; concurrency 32 is 64 requests with 32 in flight. One run per config. Throughput is output tokens a second; per-stream decode speed comes from the median time per output token (TPOT).

## Method (in brief)

- Levers, all built into vLLM 0.32.0: the official block-wise FP8 weights, an FP8 KV cache and MTP speculative decoding (3 draft tokens). On top, a third-party sparse prefill kernel (FlashPrefill V1) patched into the 16 full-attention layers.

## Limits

- Honest limit: I have not made my own quantized checkpoint. No calibration, AWQ, GPTQ or NVFP4 export. I used published checkpoints and runtime casts, and an LTX-2.3 NVFP4 attempt failed at load. Closing that gap is the obvious next piece of work.

## What this covers

- **vLLM speed-up brief: built-in levers, picked and validated**: Which built-in vLLM levers I picked for a 27B hybrid model, how I validated them against a BF16 baseline, and exactly how it was measured.
- **Inference eval lab (vLLM)**: Served a 27B model on one GPU and measured every speed lever against predictions written beforehand.
- **Quantization and compression: deploy, validate, measure**: Running quantized models in production and measuring what each format costs in speed, memory and, for video, visual quality.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
