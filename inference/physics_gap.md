# Physics-gap method: compare against the theoretical limit, then close the gap

A very simple framework: work out the theoretical limit for each step from first principles, measure the real time, rank the gaps, and improve against them one by one, kill-testing each suspected cause before building a fix.

## Results

| Measured result |
|---|
| **In the MTP step:** a cuBLAS tile mis-pick costs 1.52 ms and reading the BF16 lm_head 3x costs 4.76 ms |
| about 6 ms of a 29 ms step sized for two serving-path fixes |

## Setup and how it was measured

- Setup: one RTX PRO 6000 Blackwell Max-Q (300 W), vLLM 0.32.0, Qwen3.8-27B. Ideal times come from bytes moved at the 1.792 TB/s spec bandwidth and compute at peak; measured times come from torch-profiler traces, which match the unprofiled bench TPOT (20.88 vs 21.04 ms).

## Method (in brief)

- Method: derive the ideal time for each step from first principles (bytes moved vs memory bandwidth, compute vs peak), measure the real time, and rank the gaps.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
