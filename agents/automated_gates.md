# Automated gates, tested like code

A validator that runs every executable gate against known-bad (red) and known-good (green) fixtures, plus a mutation self-test, so each gate is tested before it guards real work.

## Results

| Measured result |
|---|
| 71 executable gates, 0 mutation survivors |
| a 193-gate spec with 921 red and 128 green mutation-test fixtures (test coverage, not detection accuracy) |

## How it works

- 71 executable gates, 0 mutation survivors: a mutation self-test (delete an operand, swap all/any, negate a comparison) must find zero survivors on the executable gates, and a ratchet only lets that bar tighten. The full spec is 193 gates across 18 check types, with 921 red (must fail) and 128 green (must pass) mutation-test fixtures; those measure test coverage, not detection accuracy. A missing value reads as MISSING and can never pass.
- Cheap judge, then strong judge: free code checks first, then a Decisions API call at a 0.5 threshold, and only flagged plans go to a stronger model that answers ok or fix. Any error is a hold. On 35 plans the first wording flagged 32 and the revised wording flagged 12; the strong model confirmed 2 as real fixes, for $0.0167 in total.
- Two-stage anatomy check: the Decisions call only routes (arm behind back, fewer than two arms visible, hand hidden by body). A trigger at 0.4 or above sends the picture to a stronger judge that reads the file and must return strict JSON; an error or non-JSON answer is a hold, never a pass. The old check scored a broken arm at 0.05 and its hidden-arm triggers fired on 35 of 110 normal poses; the two-stage gate caught the broken picture 3 of 3 and flagged 4 of 110 approved pictures.
- Frozen parity eval: the eval file is pinned by hash and every gate test has a mutation twin (patch the gate away and the test must fail). A golden re-render must match with zero error. The latest stored run failed (982 frames differing, and several tests erroring on a full disk), so it is a working gate, not a green badge.
- Hook cadence lock: at least 3 picture changes by 5.0 seconds and no picture held longer than 2.0, checked at plan time and again at render on the real voice timings, before music and render run.
- Ship gate: 17 named upload rules (frame rate, duration, loudness, reuse, target account and more), and shipping fails closed without a phone-QA receipt bound to the final file's hash from a reviewer session different from the renderer's.
- Compliance gate: PASS, REVISE or BLOCK with risk bands at 25 and 60. The publish router needs a PASS and a matching source fingerprint, so edited copy needs a fresh run. Fact-check fails closed on any high issue; scene-legal downgrades unsafe scenes to safe defaults.
- Admission gate: the rules exist twice, as Python and as gate expressions, and a test runs one table of worlds through both and fails on any difference. A mode can only be made stricter.
- Prep gate: verdicts RED-MISSING, RED-RULE, UNLOCKED, RED-DRIFT or GREEN, with rules including a hash check of 90 panels. Only GREEN exits 0, approval rejects too-short operator quotes and any drift since the proposal, and there is no flag or test hook that turns a RED into a GREEN.

---

Part of my [GitHub profile](../README.md), chapter: System design.
