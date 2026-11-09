# GPU fleet: buying agents that race the cheapest boxes

Agents that rent the cheapest GPU boxes in parallel, keep the first that proves it works, shut the rest down, and overseers that keep spend low.

## Results

| Measured result |
|---|
| Up to 10 live candidates per wave, 3 waves, first-proven instance wins |
| 12+ launchers, 6 skill-level overseers |

## What this covers

- **GPU buying agents: race the cheapest boxes, keep the first that works**: Agents rent 5 to 10 of the cheapest matching GPUs, keep the first one that proves it works, shut the rest down, and clean up anything left running, so spend stays bounded.
- **Vast.ai fleet: launchers that keep GPU spend low**: Launchers and overseers that bring GPU boxes up at the cheapest rate, run the work, and tear them down the moment it ends, so nothing bills idle.

---

Part of my [GitHub profile](../README.md), chapter: MLOps and GPU fleet.
