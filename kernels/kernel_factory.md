# Kernel factory: 2x #1, #4, #8 and #9 on GPU MODE boards, 35/35 LeetGPU Hard

Hand-tuned GPU kernels built by a kernel factory (agents, physics hypotheses and kill-tests in one loop). They reached #1 on H100 prefix sum (Oct 2026) and B200 histogram (9.27 µs, 12 Nov 2026), #4 on B200 prefix sum (315.7 µs, 10 Nov 2026), top 10 on five boards (#8 matmul H100, Oct 2026; #9 vector sum H100): solo, against team entries. They also cleared 35/35 Hard on LeetGPU in a day.

## Results

| Measured result |
|---|
| **GPU MODE:** two #1s on the global kernel leaderboards, solo: H100 prefix sum (Oct 2026) and B200 histogram (Nov 2026), edging out team entries including the Nebius Applied AI Team. Top 10 on five boards in all: #4 B200 prefix sum (315.7 µs, full precision), #8 matmul H100 (Oct 2026), #9 vector sum H100 |
| **Kernel factory:** 35/35 Hard on LeetGPU (correctness-checked on a T4) |
| **LeetGPU:** 35/35 Hard accepted in one day (correctness-checked on a T4) |

## Setup and how it was measured

- Setup: GPU MODE runs and times every submission on its own H100 runner and ranks by that timing; bandwidth share is bytes moved over time against the H100's 3.35 TB/s.

## How it works

### Kernel factory

- Scientific loop: write a physics hypothesis (bytes moved vs the memory floor, latency chains), change one thing, measure with time stamps and paired sweeps, keep only what wins, log every step.
- Method: built on the approach frontier maths researchers use with AI agents to attack hypotheses: generate a candidate, test it against a hard falsifier, keep only what survives (the same shape as agent-driven search loops used on open maths problems).
- Trading-system design: treat the gap between measured time and the physics floor as an anomaly to target; size each bet by expected gain; run many small reversible experiments; cut losers fast; stack the winners (knobs that gain only in combination).
- Kill-test before build: every hypothesis gets a one-row falsifier in an existing sweep before anyone builds it. Example: a "no-MMA x-ray" at 64.5 µs vs about 103 µs ended the multicast idea. A wait audit (kr_physics) auto-files kill-test bets in their own lane. The eval lab does the same, with a kill-test listed for each gap.
- Example, prefix sum on H100: measured stages from 1,735 µs to 532 µs, reaching #1 in the world in October 2026, at about 90% of the card's 3.35 TB/s memory bandwidth, so the memory bus is the wall. The full-precision kernel is 719 µs at 89% of bandwidth (#2 today). Biggest single step: one flag per 128-byte line to kill false sharing (−35.6 µs).
- Kernel arsenal: a catalogue of 114 distinct speed-up techniques (45 measured, 69 untested, 9 banned) deduped from 316 sources, behind a database of about 2,620 rows of variants, controls and rules. A weekly paper scout adds new ones.
- Scaled out for LeetGPU: one physics brief per problem (T4 limits), each kernel checked against a float64 NumPy twin with zero register spills; 35/35 Hard in a day.
- My role: set targets and constraints, wrote the physics framing, ruled on trade-offs, killed dead ends and reviewed every claim.
- Physics-gap report plus a busy-wait audit that automatically files kill-test bets (kr_physics, on the workstation VM).

### GPU kernels

- Speed-ups (leaderboard time vs the reference kernel; the board scores the geometric mean over its ranked shapes): Grayscale 4.98×, Prefix sum 3.26× (1,735 → 532 µs), Vector sum 1.57×, Matmul 1.15×.
- Speed: #4 on the H100 prefix-sum board within a day and a half of my first run (11–13 Oct 2026), and later #1 (22 Oct). The #1 variant was found by a sweep on day 2 but only cut into a submission after a later audit, which is now a pipeline rule.
- Reached #1 in the world on prefix sum (H100) in October 2026, on a global leaderboard open to anyone; the full-precision kernel is 719 µs at 89% of bandwidth (#2 today), and I took a second #1 on B200 histogram (Nov 2026), top 5 on one more board and top 10 on three more.
- Method: profile, try a variant, keep the faster one; the leaderboard decides.

### LeetGPU

- 39 problems accepted on Tesla T4 on 3 Nov 2026: all 35 Hard plus 4 Medium, each passing all hidden tests.
- Hard set includes flash-decoding attention, paged chunked-prefill attention, multi-head latent attention decode, speculative tree attention, fused MoE SwiGLU, NVFP4 and FP8 block-scaled GEMM, Mamba-2 SSD scan, FFT, radix sort and full GPT-2 and Llama transformer blocks.
- Method: mapped all 147 challenges to my kernel arsenal (75 covered, 38 partial, 34 gaps) and seeded 16 new techniques for the gaps; one brief per problem with T4 speed-of-light physics (40 SMs, 320 GB/s); parallel builder agents compiled for sm_75/80/90 with zero register spills and checked each kernel against a float64 NumPy twin; submitter agents submitted, one tab each.

## Limits

- Caveats: correctness only; not claimed fastest. Runtimes were shown for only some problems (e.g. sliding-window attention 0.12 ms, FFT 2.49 ms, radix sort 37.72 ms), so closeness to the T4 limit is unmeasured for most.

## What this covers

- **Kernel factory: the system behind two #1s on GPU MODE**: Agents, physics hypotheses and kill-tests run as one loop: the factory behind two #1s on GPU MODE, solo (H100 prefix sum, Oct 2026; B200 histogram, Nov 2026), top 10 on five boards in all and 35/35 Hard on LeetGPU.
- **GPU kernels (GPU MODE)**: Hand-tuned GPU kernels: two #1s on GPU MODE's global kernel leaderboards, solo (H100 prefix sum, Oct 2026; B200 histogram, Nov 2026), edging out team entries including the Nebius Applied AI Team; top 10 on five boards in all.
- **LeetGPU: 35/35 Hard kernels in a day**: Cleared all 35 of 35 Hard LeetGPU challenges in one day, correctness-checked on a T4, with a kernel factory of builder and submitter agents.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
