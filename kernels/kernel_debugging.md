# Kernel debugging: a drill factory and the real incidents behind it

A debug factory with one canonical step graph, seeded from real incidents, plus the incidents themselves: symptom, root cause, fix.

## Results

| Measured result |
|---|
| 30 failure classes, 22 symptom signatures, a fixed 6-step graph |
| **all 50 lab drills:** right class and right kernel on 46, 0 false confirms |
| 8 real incidents |
| a "cache-off" baseline that was secretly 45% cached |
| a stall that was really a 13% slower clock |
| sparse prefill 1.44× at 64k once 3 memory walls were cleared (8/10 needle recall at 128k, full quality gate pending) |

## Method (in brief)

- Design: inspired by airplane checklists and Warren Buffett's discipline: one canonical way to execute, fired off the same way every time, so pressure and emotion never pick the next step.
- Stages: classify first (read the symptoms and rank the likely failure classes), then localise (which kernel fails, and how: errors, unwritten, wrong or non-finite values, nondeterminism), then re-score the ranking on that evidence, then run the checks in expected-value order (probability × impact ÷ cost to check) and stop early once one class clearly leads, then bisect against the last good version and prove the fix with a red/green regression test.
- Structure: one entry point and one source of truth. The bank of classes, signatures and cases is read as a stamped, read-only snapshot; the engine refuses anything unstamped, appends solved cases only, and refuses to write if the bank changed mid-run.

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
