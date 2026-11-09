# Kernel factory: #1 on GPU MODE, top 10 on two more boards, 35/35 LeetGPU Hard

Hand-tuned GPU kernels built by a kernel factory (agents, physics hypotheses and kill-tests in one loop). They reached #1 on the GPU MODE prefix-sum board (H100, Oct 2026) and the top 10 on two more boards, and cleared 35/35 Hard on LeetGPU in a day.

## Results

| Measured result |
|---|
| **GPU MODE:** reached #1 on the H100 prefix-sum board (Oct 2026), 532 µs with a 16-bit output the grader accepted |
| the full-precision kernel is 719 µs at 89% of bandwidth (#2 today) |
| #9 matmul H100, #9 vector sum H100 |
| 35/35 Hard on LeetGPU (correctness-checked on a T4) |
| 35/35 Hard accepted in one day (correctness-checked on a T4) |

## Setup and how it was measured

- Setup: GPU MODE runs and times every submission on its own H100 runner and ranks by that timing; bandwidth share is bytes moved over time against the H100's 3.35 TB/s.

## Method (in brief)

- How it was done: built with the kernel factory (agents plus physics hypotheses plus kill-tests) and learned through the lexicon factory; see both.
- Method: profile, try a variant, keep the faster one; the leaderboard decides.
- Method: mapped all 147 challenges to my kernel arsenal (75 covered, 38 partial, 34 gaps) and seeded 16 new techniques for the gaps; one brief per problem with T4 speed-of-light physics (40 SMs, 320 GB/s); parallel builder agents compiled for sm_75/80/90 with zero register spills and checked each kernel against a float64 NumPy twin; submitter agents submitted, one tab each.

## Limits

- Caveats: correctness only; not claimed fastest. Runtimes were shown for only some problems (e.g. sliding-window attention 0.12 ms, FFT 2.49 ms, radix sort 37.72 ms), so closeness to the T4 limit is unmeasured for most.

## What this covers

- **Kernel factory: the system that reached #1 on GPU MODE**: Agents, physics hypotheses and kill-tests run as one loop: the factory that took a prefix-sum kernel to #1 on GPU MODE (H100, Oct 2026), top 10 on two more boards and 35/35 Hard on LeetGPU, with every mechanism first learned through the lexicon factory.
- **GPU kernels (GPU MODE)**: Hand-tuned GPU kernels: reached #1 in the world on the GPU MODE prefix-sum leaderboard (H100, Oct 2026), a global board open to anyone.
- **LeetGPU: 35/35 Hard kernels in a day**: Cleared all 35 of 35 Hard LeetGPU challenges in one day, correctness-checked on a T4, with a kernel factory of builder and submitter agents.

---

Part of my [GitHub profile](../README.md), chapter: GPU kernels.
