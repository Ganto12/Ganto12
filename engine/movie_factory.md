# Movie factory: a film as a gated graph

A long-form film pipeline built as a fixed graph of stages, each behind a quality gate, with every choice recorded as data.

## Results

| Measured result |
|---|
| 34-node graph with 7 loops, 193 gates, 42 quality dimensions |
| passed four clean-context audits |

## Method (in brief)

- Structure: a fixed graph of 34 nodes and 7 loops, read as three blocks, with a gate at every step. Gates, axes, routes and banned phrases live in data files, so the validator can run every gate against known-bad and known-good test cases.
- Modular axes and harnesses: the 42 quality dimensions live in data files beside the gates and routes, so a new film is a new set of axis values rather than new code.

---

Part of my [GitHub profile](../README.md), chapter: Content engine.
