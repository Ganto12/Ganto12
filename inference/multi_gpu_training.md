# Multi-GPU training and serving: 70B fine-tunes on 2× B200, then letting a classifier win

Fine-tuning up to 70B on multi-GPU boxes, tensor-parallel serving, simulated larger layouts, and the result that a small classifier matched the 70B model on the task.

## Results

| Measured result |
|---|
| 70B LoRA trained on 2× B200 (1,500 steps) and served tensor-parallel 2 in vLLM |
| 7+ models on 8 GPU types across Vast and Verda |
| DeepSeek 70B reached 2.6 s error per shot, then an XGBoost classifier matched it at 2.67–2.86 s |
| **70B LoRA on 2× B200:** 1,500 steps, loss 1.86 → 0.036 |
| 7+ models |
| configs from A40 to B200, B300 and 8× H200 |
| **Real:** 70B served tensor-parallel 2 on 2× B200 |
| **simulator, not validated against a measured point:** a second Llama-8B replica raised throughput 7,098 → 10,183 tok/s (under overload, mean TTFT 6.6 s → 87 ms) |
| kwok packed 7 of 7 GPU jobs |

## Method (in brief)

- How I got to owning the stack: free notebooks on Kaggle first (Google's GPUs were too weak for video), then hosted Jupyter notebooks on a Paperspace / DigitalOcean subscription with 48 GB A6000-class cards, where the billing was poor value.
- Rented, not owned: GPUs from Vast and Verda (A4000, A40, A100, H100, RTX PRO 6000, B200, B300 and H200), single cards up to 8-GPU nodes, with Docker GPU passthrough on Verda through the NVIDIA Container Toolkit.
- The fallback that won: an XGBoost cut classifier with a dynamic-programming decoder came in at 2.67–2.86 s per shot for a fraction of the cost and complexity, so it replaced the fine-tuned model and became QuantEditor.

## Limits

- What broke, and why: MiniMax-M2.1 229B QLoRA with ZeRO-3 on 4× A40 failed, as on 2× A100, because the model is FP8-native and needs compute capability 9.0 or higher; some B300 runs were blocked because Triton had no sm_103a support yet; a pre-flight NVLink check in the multi-GPU orchestrators found 2× A100 with NVLink inactive while 2× H100 had all 18 links active.
- Lesson: 2 GPUs are not 2×. Splitting one model pays a sync cost after every layer, and once a model fits one card, cutting bytes or steps beats adding GPUs. Spent before deciding training wasn't needed: about £250–500 of GPU time (my estimate).
- Lesson from that run: the default rolling update deadlocks on full GPUs, because the new pod cannot fit while the old one still holds the card. Setting no surge (replace one at a time) fixed it.

## What this covers

- **Multi-GPU fine-tuning up to 70B, then letting a classifier win: the road to QuantEditor**: Fine-tuned 7+ open models for shot-length prediction on rented multi-GPU nodes (DeepSpeed ZeRO-3 sharding, torchrun DDP, vLLM tensor parallel on 2× B200), paid the 2-GPUs-are-not-2× tax first-hand, then shipped a small classifier when it matched the 70B.
- **LLM fine-tuning & training pipeline**: Config-driven LoRA/QLoRA fine-tuning on single GPUs and multi-GPU nodes: DeepSpeed ZeRO-3 sharding, torchrun DDP and NCCL tuning, with one YAML per model, GPU and method.
- **Multi-GPU serving: real tensor-parallel runs, then simulated layouts and Kubernetes GPU scheduling**: Built on real multi-GPU work (ZeRO-3 and DDP training, a 70B served tensor-parallel 2 on 2× B200), then simulated the layouts I could not rent, on profiles measured from a real Blackwell card, plus Kubernetes GPU scheduling in kwok.

---

Part of my [GitHub profile](../README.md), chapter: Inference and training.
