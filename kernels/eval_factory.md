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

## How it works

- Steps: 0 determinism floor (the base must reproduce itself bit-for-bit first); 1 bit check (SHA-256 of the full logits at random split points and odd or prime tails); 2 token checks (greedy, a 1,024-token long decode, seeded sampling); 3 perplexity, KL divergence and top-1 agreement; 4 canonical long-context tasks; 5 speed; 6 verdict.
- Long-context step: the evals labs report in 2026 model cards, OpenAI MRCR v2 8-needle and GraphWalks, at pinned dataset revisions with their official graders, a frozen subset and paired bootstrap confidence intervals.
- Speed step: base and candidate runs are interleaved; it reports median and p90, peak memory and cache state, and the timed runs must hash-match the checked outputs, so speed is never measured on a path that was not checked.
- Rigor rules: EQUIVALENT needs an equivalence test (the whole 95% CI inside the margin); a wide CI that merely contains zero is INCOMPLETE, never a pass; any missing gate is INCOMPLETE; every failure is reported.
- Transparency: the reference step was added after a first result came out LOSSY on absolute thresholds; the config's change log records it, and earlier reports keep their original verdict with the new one shown beside it.
- Reference distance (step 3.5): when base and candidate differ, both are scored against one common reference forward pass, the engine's fixed-order CPU reference with 32-bit accumulation (intermediate values stored in the model's own 16-bit format, so a clean reference implementation, not a higher-precision model; the same reference the engine's own tests use). A noise floor measures how far the base drifts from itself when the same prompt is batched differently; the candidate passes a non-inferiority test if the paired 95% CI upper bound of its extra KL to the reference is within that floor. Every top-1 flip must be at a near-tie (top-1 minus top-2 logit gap under 1.0); a wide-margin flip is a bug, never noise. Tokens to first divergence are reported. When bits differ, this step decides EQUIVALENT.
- Wiring: the kernel debug factory (step 7) and the kernel autoresearch factory hand every numerics-touching change to it.
- Proof so far: on synthetic records it returns LOSSLESS, EQUIVALENT, LOSSY and INCOMPLETE in the right cases. Its first real use, a persistent prefix cache for a local Apple Silicon inference engine, came back LOSSLESS on both model families tested.
- What it does: saves the model's full prefilled state to disk, the attention KV plus the hybrid models' recurrent state (DeltaNet and convolution layers), so a new session or process restores it instead of re-reading the prompt.
- Lossless on Qwen 3.5 0.8B (DeltaNet hybrid) and a second hybrid model (convolution + attention): restored internal state hashes byte-identical to a fresh prefill; logits bit-identical on 100/100 probes; greedy, 1,024-token long-decode and seeded-sampling outputs identical; perplexity identical.
- Safe by design: corrupt, truncated, wrong-model, wrong-device, wrong-version, wrong-kernel-build and too-small-context snapshots always miss and fall back to a normal prefill, 10 of 10 cases on both models. Snapshot files are owner-only, and two saves of the same prompt are byte-identical.
- Per-size detail, cold-disk restore (snapshot forced out of the OS file cache before every timed restore), median of 15 runs per size, cache at 1,024-token boundaries: Qwen 3.5 0.8B 6.5x at 1,024 tokens, 14.3x at 2,048, 26.6x at 4,096, 45x at 8,192; an unaligned 1,499-token prompt gets 2.7x. Measured on the final code after the last safety fixes. The second model's numbers are noisier at short lengths, so they are not quoted.
- On the same laptop, MLX and llama.cpp prompt caching also restore the model correctly; restored time to first token was 49-89 ms (ours), 75-120 ms (MLX) and 102-146 ms (llama.cpp) at 1K to 8K tokens, each on its own quantisation. Not a like-for-like ranking: quantisation, prompts and timers differ slightly between engines.
- Scope: times exclude model load and the one-off save; Apple M1 Pro only, MXU chips (M5, A19) untested. Two independent adversarial audits (code, and evidence and claims) had every P0 fixed first; every run is in an append-only ledger; final numbers come from a frozen probe corpus.
- Profiles, so each change is judged the right way: state (caches, snapshots, KV paging: byte-identical internal state, safety negatives where corrupt or skewed files must miss, bits, tokens, perplexity, speed); numerics (kernel swaps, batch invariance: bits may differ, the common-reference step decides); lossy (quantization, KV compression: tasks mandatory, with enough samples for the margin).
- Gates added after two independent audits of my own work: state identity (hash of restored KV and recurrent state), safety negatives, and a speed headline that is a per-size table (15+ interleaved runs, a CI per size, logged power, thermal and cache state, end-to-end wall clock and footprint; geomean secondary). Verdicts are scoped to the models and chips tested; thresholds that fail the base against its own noise floor are flagged as miscalibrated.
- Locked like a pre-registered study: required metadata, profile must fit the change, config hash checked, every attempt in an append-only ledger, no overrides.
- Self-test: the eval factory tests itself. 23 known-answer cases must pass for the exact code and config before it issues any verdict, and each rule is mutation-tested (disabling any one makes the self-test fail). Inputs must be frozen, registered corpora: drifting, mixed or tampered inputs return INVALID, and duplicate-probe padding is rejected.
- Opt-in batch-invariant prefill: bit-exact restores from 64-token snapshots (confirmed cold), so prompts that are not a multiple of 1,024 tokens can still restore from a nearby snapshot: the 1,499-token prompt goes from 2.7x to 8.6x, with no detectable cost (CI about ±15%). It changes outputs relative to the default kernels; against a CPU reference the difference was not significant on 48 short probes.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
