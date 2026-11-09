# Eval factory: is a faster path still the same model?

A locked graph that decides whether a change to a model's inference path (kernel swap, cache, precision, fusion, batching) is lossless, equivalent, lossy or incomplete before any speed claim.

## How it decides

- **Locked steps:** a determinism floor (the base must reproduce itself bit for bit), a bit check on the full logits, token checks (greedy, long decode, seeded sampling), perplexity, KL divergence and top-1 agreement, long-context tasks, then speed, with every timed run hash-matched to a checked output.
- **Different but valid numerics:** when outputs differ, both sides are scored against one common reference with a noise floor and a non-inferiority margin, the way quantization research does it.
- **Locked like a pre-registered study:** frozen inputs, a checked config hash, an append-only ledger and no overrides. A 23-case self-test, each rule mutation-tested, must pass before it issues any verdict.

## The same gates across the stack

- In the agent harnesses, sub-agents act as judges: free code checks first, then a cheap model, then a stronger one, and any error is a hold, never a pass (see [automated gates](../agents/automated_gates.md)).
- The movie factory runs 193 gates over a 34-node graph, each tested against known-bad and known-good cases, and passed four clean-context audits (see [movie factory](../engine/movie_factory.md)).

## Results

| Measured result |
|---|
| Locked steps 0–6 plus a reference-distance step |
| 4 verdicts |
| long-context checks on MRCR v2 8-needle and GraphWalks with paired bootstrap CIs |
| **first real use:** a persistent prefix cache, LOSSLESS on 2 model families, cold restore 6.5x faster time to first token at 1,024 tokens and 45x at 8,192 (M1 Pro) |

---

Part of my [GitHub profile](../README.md), chapter: GPU kernels.
