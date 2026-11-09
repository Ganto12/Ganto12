# A little model training: 70B LoRA fine-tunes on 2× B200, then letting a classifier win

Fine-tuning up to 70B on multi-GPU boxes, tensor-parallel serving, simulated larger layouts, and the result that a small classifier matched the 70B model on the task.

## Results

| Measured result |
|---|
| **Multi-GPU fine-tuning up to 70B, then…:** 70B LoRA trained on 2× B200 (1,500 steps) and served tensor-parallel 2 in vLLM |
| **Multi-GPU fine-tuning up to 70B, then…:** 7+ models on 8 GPU types across Vast and Verda |
| **Multi-GPU fine-tuning up to 70B, then…:** DeepSeek 70B reached 2.6 s error per shot, then an XGBoost classifier matched it at 2.67–2.86 s |
| **70B LoRA on 2× B200:** 1,500 steps, loss 1.86 → 0.036 |
| **LLM fine-tuning & training pipeline:** 7+ models |
| **LLM fine-tuning & training pipeline:** configs from A40 to B200, B300 and 8× H200 |
| **Real:** 70B served tensor-parallel 2 on 2× B200 |
| **simulator, not validated against a measured point:** a second Llama-8B replica raised throughput 7,098 → 10,183 tok/s (under overload, mean TTFT 6.6 s → 87 ms) |
| **Multi-GPU serving:** kwok packed 7 of 7 GPU jobs |

## How it works

### Multi-GPU fine-tuning up to 70B, then…

- The task: predict each shot's length, with reasoning, from the previous 3 shots, words per minute and a creator style guide. The pilot had 1,403 shots; the 10-video set had 3,043 samples.
- How I got to owning the stack: free notebooks on Kaggle first (Google's GPUs were too weak for video), then hosted Jupyter notebooks on a Paperspace / DigitalOcean subscription with 48 GB A6000-class cards, where the billing was poor value. Then Vast.ai: rent exactly the GPU I need, SSH in, run my own stack and pay only while it runs. Far more flexible, and it is where the fleets, baked images and race launchers came from.
- Rented, not owned: GPUs from Vast and Verda (A4000, A40, A100, H100, RTX PRO 6000, B200, B300 and H200), single cards up to 8-GPU nodes, with Docker GPU passthrough on Verda through the NVIDIA Container Toolkit.
- Real multi-GPU training, step by step: from a 7B 4-bit LoRA on one A4000 (6.15 s a step), to a naive device_map split of one model across 2 cards, to DeepSpeed ZeRO-3 (parameters, gradients and optimizer state sharded across GPUs, with CPU offload; configs for 4× A40 and A6000), to torchrun DDP with a full model copy per GPU and NCCL peer-to-peer and InfiniBand turned off to stop deadlocks on Vast. Nemotron-30B on 2× RTX PRO 6000 ran 25–26 s a step for 1,400 steps.
- Models: LoRA and QLoRA adapters on 4-bit and 16-bit bases for Qwen3 14B and 32B, Nemotron 30B, DeepSeek-R1-Distill 8B and 70B (the 70B on 2× B200 for 1,500 steps, with configs for 1× B300, 8× B200 and 8× H200) and others, plus an Unsloth-to-standard merge for Nemotron.
- Serving the fine-tuned 70B: vLLM tensor parallel 2 across 2× B200 (FlashInfer backend) for checkpoint evals; the eval plan scaled to 8× H200 at tensor parallel 8, and the Verda vLLM labelling factory sets tensor parallel to the GPU count (8× H100 by default), with data parallel across boxes.
- Result: DeepSeek 70B reached 2.6 s mean error per shot, 43% within 1 s; Nemotron reached 2.95 s.
- The fallback that won: an XGBoost cut classifier with a dynamic-programming decoder came in at 2.67–2.86 s per shot for a fraction of the cost and complexity, so it replaced the fine-tuned model and became QuantEditor.

### LLM fine-tuning & training pipeline

- Qwen3 14B/32B, DeepSeek-R1 8B/70B Distill, DeepSeek V3/V3.2, Nemotron 30B, GLM-4 7B, MiniMax-M2.1.
- LoRA, QLoRA, torchrun DDP, DeepSpeed ZeRO-3 (sharded parameters, gradients and optimizer state), Flash Attention 2; per-(model, GPU, method) YAML configs; a pre-flight NVLink check in every multi-GPU orchestrator; GPU hunting across Vast, Verda and Paperspace.
- Finished multi-GPU run: DeepSeek-R1-Distill-Llama-70B LoRA on 2x B200 (as configured for the run), 1,500 steps over 8.5 epochs, loss 1.86 to 0.036 on a 30-video set, which most likely means overfitting. Checkpoints were saved every 500 steps and backed up to B2.
- Honest gap: the B2 copies hold only the LoRA adapter (3.3 GB for the 70B run), not optimizer, scheduler or RNG state. They are good for evaluation and warm starts, not for an exact resume. A full resumable state is the next thing to prove.
- Scope, stated plainly: every run was LoRA or QLoRA adapter training on top of a frozen base model, through the Hugging Face Trainer. None was full fine-tuning or pretraining.

### Multi-GPU serving

- The real part: the multi-GPU training and serving runs in the training row (ZeRO-3 and DDP training, a 70B served tensor-parallel 2 on 2× B200, NCCL deadlocks fixed). The layouts below are LLMServingSim runs on a CPU-only VM, using measured RTX PRO 6000 Blackwell profiles and PCIe collective timings; they are simulated (simulator, not validated against a measured point), and they are not H100/B200/B300 numbers.
- Llama-3.1-8B: 2 replicas raised throughput 7,098 → 10,183 tok/s and removed the queue (under overload, mean first-token 6,560 → 87 ms); 4 × tensor-parallel 2 on 8 GPUs cut per-token latency by a third.
- Qwen3.8-27B: prefill/decode split cut P99 first-token 46 s → 7.9 s but made per-token latency 47% worse. MiniMax-M3 on 16 GPUs: data-parallel attention + expert-parallel 16 cut per-token latency 42% and raised throughput 33%, at about 2× first-token.
- Odd results kept on purpose: Qwen tensor-parallel 2 came out slower than one GPU (suspicious); DeepSeek prefill/decode comparisons did not finish; expert-routing policies showed no effect.
- Ties to the lesson from my own training runs: 2 GPUs are not 2×.
- Kubernetes GPU scheduling (kwok, run on the workstation, every component bound to localhost): 8 fake GPU nodes (4 RTX PRO 6000, 4 H100) with 8 GPUs each. Scheduled 24 single-GPU vLLM replicas and 12 two-GPU tensor-parallel replicas pinned to H100 nodes: all 36 pods placed within about 9 seconds, 48 of 64 GPUs allocated. A 16-GPU pod correctly stayed pending, since no single node holds 16 GPUs.
- Rerun with my real container images and measured GPU memory as a schedulable resource: 3 RTX PRO 6000 (96 GB) and 2 RTX 5090 (32 GB) nodes; MiniMax H3 at 54 GB (480p) and 61 GB (768p), SeedVR2 at 30 GB tiled and 37 GB untiled, LTX HighQ and distilled each needing a whole 96 GB card. With a preference sending tiled SeedVR2 to the 5090s, 7 of 7 jobs packed (Pros at 91, 91 and 54 of 96 GB); the LTX jobs waited for a free card.

## Limits

- What broke, and why: MiniMax-M2.1 229B QLoRA with ZeRO-3 on 4× A40 failed, as on 2× A100, because the model is FP8-native and needs compute capability 9.0 or higher; some B300 runs were blocked because Triton had no sm_103a support yet; a pre-flight NVLink check in the multi-GPU orchestrators found 2× A100 with NVLink inactive while 2× H100 had all 18 links active.
- Lesson: 2 GPUs are not 2×. Splitting one model pays a sync cost after every layer, and once a model fits one card, cutting bytes or steps beats adding GPUs. Spent before deciding training wasn't needed: about £250–500 of GPU time (my estimate).
- Lesson from that run: the default rolling update deadlocks on full GPUs, because the new pod cannot fit while the old one still holds the card. Setting no surge (replace one at a time) fixed it.

## What this covers

- **Multi-GPU fine-tuning up to 70B, then letting a classifier win: the road to QuantEditor**: Fine-tuned 7+ open models for shot-length prediction on rented multi-GPU nodes (DeepSpeed ZeRO-3 sharding, torchrun DDP, vLLM tensor parallel on 2× B200), paid the 2-GPUs-are-not-2× tax first-hand, then shipped a small classifier when it matched the 70B.
- **LLM fine-tuning & training pipeline**: Config-driven LoRA/QLoRA fine-tuning on single GPUs and multi-GPU nodes: DeepSpeed ZeRO-3 sharding, torchrun DDP and NCCL tuning, with one YAML per model, GPU and method.
- **Multi-GPU serving: real tensor-parallel runs, then simulated layouts and Kubernetes GPU scheduling**: Built on real multi-GPU work (ZeRO-3 and DDP training, a 70B served tensor-parallel 2 on 2× B200), then simulated the layouts I could not rent, on profiles measured from a real Blackwell card, plus Kubernetes GPU scheduling in kwok.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
