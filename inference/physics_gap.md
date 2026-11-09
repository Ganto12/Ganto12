# Physics-gap method: compare against the theoretical limit, then close the gap

A very simple framework: work out the theoretical limit for each step from first principles, measure the real time, rank the gaps, and improve against them one by one, kill-testing each suspected cause before building a fix.

## Results

| Measured result |
|---|
| **In the MTP step:** a cuBLAS tile mis-pick costs 1.52 ms and reading the BF16 lm_head 3x costs 4.76 ms |
| about 6 ms of a 29 ms step sized for two serving-path fixes |

## Setup and how it was measured

- Setup: one RTX PRO 6000 Blackwell Max-Q (300 W), vLLM 0.32.0, Qwen3.8-27B. Ideal times come from bytes moved at the 1.792 TB/s spec bandwidth and compute at peak; measured times come from torch-profiler traces, which match the unprofiled bench TPOT (20.88 vs 21.04 ms).

## How it works

- Method: derive the ideal time for each step from first principles (bytes moved vs memory bandwidth, compute vs peak), measure the real time, and rank the gaps. Then give each gap a one-row kill-test that confirms or kills the suspected cause before anyone builds a fix.
- Decode (C3: FP8 weights + FP8 KV, batch 1): 15.21 ms ideal vs 20.88 ms measured, 72.8% of spec-bandwidth speed-of-light (1.792 TB/s). Cold 250k-token prefill: 39.6% of the physics limit.
- Finding (C6, the MTP step, 4 tokens per verify): a cuBLAS tile mis-pick on GDN in_proj_ba costs 1.52 ms per step (1.55 ms measured vs about 26 µs ideal). At 4 tokens cuBLAS leaves the gemv path and picks a wmma tile with about 6 CTAs on 188 SMs.
- Cost, not a find: in the same MTP step, reading the BF16 lm_head 3 times (once per draft token) costs 4.76 ms, about 17% of the step. That is how MTP drafting works. The kernel already runs at 89% of bandwidth, so the lever is fewer bytes (a pruned draft vocab or an FP8 lm_head).
- Together about 6 ms of a 29 ms MTP step (about 20%) is sized for two serving-path changes; sized, not yet landed.
- The factory's automated version: a physics-gap report plus a busy-wait audit that auto-files kill-test bets in their own lane (on the workstation VM).
- Why it matters: it replaces tuning by guesswork with a ranked list of where the time actually goes, the same discipline as measuring a trading edge before sizing it.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
