# QuantEditor: Markovian and goal-directed video editing, learned from top creators

A model that learns how top creators cut and writes the cut plan for a new script. Editing is modelled as part Markovian (each shot length depends on the last) and part teleological (a section-level cut-rate goal steers the whole path), decoded with dynamic programming.

## Results

| Measured result |
|---|
| 121 training videos, 96 tracked experiments |

## How it works

- Editing as part Markovian (shot length depends on the last) and part goal-directed (section cut-rate prior + whole-path DP decode).
- XGBoost cut classifier → DP decoder → N shots → scenes → TTS-snapped edit.
- Judged on shot-duration error and feel metrics; pruning beat adding features.

---

Part of my [GitHub profile](../README.md), chapter: Content engine.
