# Kernel debugging: a drill factory and the real incidents behind it

A debug factory with one canonical step graph, seeded from real incidents, plus the incidents themselves: symptom, root cause, fix.

## Results

| Measured result |
|---|
| **Kernel debug factory:** 30 failure classes, 22 symptom signatures, a fixed 6-step graph |
| **all 50 lab drills:** right class and right kernel on 46, 0 false confirms |
| **Debugging for real:** 8 real incidents |
| **Debugging for real:** a "cache-off" baseline that was secretly 45% cached |
| **Debugging for real:** a stall that was really a 13% slower clock |
| **Debugging for real:** sparse prefill 1.44× at 64k once 3 memory walls were cleared (8/10 needle recall at 128k, full quality gate pending) |

## How it works

### Kernel debug factory

- Design: inspired by airplane checklists and Warren Buffett's discipline: one canonical way to execute, fired off the same way every time, so pressure and emotion never pick the next step.
- Stages: classify first (read the symptoms and rank the likely failure classes), then localise (which kernel fails, and how: errors, unwritten, wrong or non-finite values, nondeterminism), then re-score the ranking on that evidence, then run the checks in expected-value order (probability × impact ÷ cost to check) and stop early once one class clearly leads, then bisect against the last good version and prove the fix with a red/green regression test.
- Accuracy rule: every check starts from a known-good condition that must pass, and counts only when one change flips it to a fail, so an already-broken output can never confirm the wrong cause.
- Structure: one entry point and one source of truth. The bank of classes, signatures and cases is read as a stamped, read-only snapshot; the engine refuses anything unstamped, appends solved cases only, and refuses to write if the bank changed mid-run. The Debug Hub page renders the same bank, so the page and the factory can never drift apart.
- The bank: 50 incident drills from my Triton lab, 30 failure classes and 22 symptom signatures. Every solved case is added back, so the priors sharpen with each rep.
- Two voices: every diagnosis is written once in kernel words (mechanism, culprit line, the check that caught it) and once in my own lexicon, so each rep deepens the understanding, not just the fix.
- Results: on all 50 lab drills it named the right failure class and the right kernel on 46, with 0 false confirms; the other 4 are left undiagnosed rather than guessed. Tightening the order (re-score after localising, early stop, checks that must flip a passing case to a failing one) cut false confirms from 3 to 0 and median time by about 40% on the first 8 drills.
- Modes: status (read-only ranking of every class at current priors) and run (diagnose incidents and append the cases); registered in FactoryOps beside the kernel factory, with its own skill, so a clean-context agent runs it the canonical way the first time.
- The method, for an engine problem like a long-context OOM: split ours vs someone else's (ours ships behind locked, deterministic evals, so a failure means the evals get audited first). Hardware first, then the locked Wrong X checklist → maths → shape → state → engine flag → capture → time → place → measurement. Find the tell, score each hypothesis by probability, and grade the urgency: parallel agents if customers are hit, one agent for a slow burn. Ship only through red/green evals at production concurrency, batch-invariant outputs and a canary.

### Debugging for real

- B200 prefix sum: a kernel passed my own audit 26 of 26, then failed the leaderboard's hidden validation. Suspected cause: the order of work inside a block is not guaranteed, so a write can race a read. The next candidate had to prove its ordering and pass a many-call full-output stress test before promotion; the 1 µs 'gain' turned out not to be real.
- B200 matmul: each pipeline stage looked 0.067 µs too slow. The model assumed 1.84 GHz, but the clock measured under load was 1.602 GHz; at the real clock a stage ran at 97% of the tensor-core rate, so there was no stall. The time was re-attributed to 10.3 µs of fixed overhead and 3.8 µs of wave quantisation.
- Kernel sweep: all 26 rows came back empty. Extra compiler flags broke the leaderboard runner's precompiled-header build, which a local compile check can't see. Flags removed, the module built, and the base row read 112.51 µs against about 113 for the reference. Rule kept: only copy flags from a module that already built on the runner.
- vLLM serving: the 'cache-off' baseline was secretly cached. The installed vLLM version ignored the config file's prefix-caching switch (467,264 of 1,040,146 prefix tokens hit). Switched to the command-line flag and kept only true cold runs: real uncached time to first token 1.93 s at 8k up to 135.65 s at 256k.
- Sparse prefill on an RTX PRO 6000: two sparse-attention servers crashed one after another on three memory walls (4.2 GiB free against a 4.25 GiB allocation, 8.5 GiB of scratch against 3.5 GiB free at 128k, and 106,496 bytes against a 101,376-byte shared-memory limit). FP8 weights (25 GB saved), lower memory utilisation and forced stages cleared them: cold prefill at 64k in 9.74 s against 13.99 s dense (1.44×), with 8/10 needle recall at 128k; full quality gate pending.
- Wan image-to-video on a rented host: every frame decoded black although sampling succeeded. Not the weights: still black with quantisation off, with verified FP8 hashes and across four attention backends. Traced to the runtime stack (framework, torch and CUDA backend versions); moved to a known-good image with a one-image smoke test before any full queue.
- Multi-GPU training: data-parallel runs deadlocked on rented hosts. Cause: NCCL peer-to-peer and InfiniBand paths on those machines; disabling both fixed it. A 30B model then trained on 2× RTX PRO 6000 at about 25.5 s per step for 1,400 steps (about 10 hours).
- Beyond the 8 (details sparse now, the patterns are what stayed):
- Wrong runtimes: model modes failing on the wrong mix of framework, torch, CUDA and attention versions. Each fix ended as one pinned runtime per mode inside a factory, so the same failure can't recur.
- Samplers and schedulers: image and video models giving soft, broken or off-style output from a sampler or scheduler that didn't match what the model was trained and tuned for; matched to the lab's settings and locked per mode.
- Custom ComfyUI workflows: community workflows (for example from Civitai) almost never ran clean out of the box, whatever way they were saved. The fix was nearly always the delta: diff the custom workflow against the lab's own reference workflow, line by line, and the broken node, setting or version shows itself.
- Audio: text-to-speech and sound-effect stages with their own failures (timing, levels, artefacts), debugged the same way and locked into their factory stages.
- Quantization for quality: FP8 and lower-bit formats checked with evals, not by eye alone, measuring what each format costs in speed, memory and visual or output quality before it is allowed into a factory.
- Kernels without a GPU: compiling and checking kernels on the CPU-only VM first caught build and logic errors before paying for any GPU time.

## Limits

- Limits: the lab runs on CPU; 11 of the 30 classes (coalescing, bank conflicts, occupancy and other performance classes) need a GPU and are not run yet, so no production time-to-diagnosis is claimed.
- Limits: story 1's cause is suspected, not proven; stories 6 and 8 have no fix number because the logs don't record one.

## Next

- Next: every solved case is labelled data (symptoms and probe results, then the class and the kernel), so a small classifier can later be trained on the bank to pick the class in one shot, with the step graph kept as the verifier.

## What this covers

- **Kernel debug factory: 50 incidents, one canonical step graph**: A checklist, not a mood: any broken or slow kernel goes through the same fixed step graph, cheapest checks first, with causes ranked by expected value, so emotion never decides where to look.
- **Debugging for real: 8 incidents and the patterns behind many more**: Real failures across kernels, serving, image, video and audio models and multi-GPU training, each traced to a mechanism, and the recurring ones locked into factories so they never come back.

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
