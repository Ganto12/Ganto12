# Physics-gap method: where the time goes, then kill-test the cause

For every serving step, compare measured time to the physics limit, find the biggest gaps, and kill-test the cause before building a fix.

## Results

| Measured result |
|---|
| Batch-1 decode at 72.8% of spec-bandwidth speed-of-light in config C3 (15.21 ms ideal vs 20.88 ms measured) |
| in the MTP step a cuBLAS tile mis-pick costs 1.52 ms and reading the BF16 lm_head 3x costs 4.76 ms (how MTP works) |
| cold 250k prefill at 39.6% of the physics limit |

## Setup and how it was measured

- Setup: one RTX PRO 6000 Blackwell Max-Q (300 W), vLLM 0.32.0, Qwen3.8-27B. Ideal times come from bytes moved at the 1.792 TB/s spec bandwidth and compute at peak; measured times come from torch-profiler traces, which match the unprofiled bench TPOT (20.88 vs 21.04 ms).

## Method (in brief)

- Method: derive the ideal time for each step from first principles (bytes moved vs memory bandwidth, compute vs peak), measure the real time, and rank the gaps.

---

Part of my [GitHub profile](../README.md), chapter: Inference and training.
