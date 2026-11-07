# AI systems that ship and scale

Solo, with parallel agents, I build GPU inference, model training and content factories, plus consumer apps.

- **#1**: in the world on the GPU MODE prefix-sum leaderboard, H100
- **1.86×**: stock vLLM batched throughput, same hardware
- **35/35**: LeetGPU Hard kernels accepted in a day
- **162bn**: tokens through coding agents: Codex 119bn + Claude Code 42.9bn

## Map

```text
Portfolio
├── Inference & systems: Faster models on the same GPUs.
│   ├── GPU kernels ................................ #1 in the world, prefix sum on H100
│   ├── vLLM speed-up brief ........................ Stock vLLM → 1.86× batched
│   ├── LeetGPU .................................... 35/35 Hard + 4 Medium accepted
│   ├── Quantization and compression ............... FP8: 50.1 → 27.6 GiB, up to 3.08× decode
│   ├── Local vs API cost arbitrage ................ Video: 116×–250× below API cost (local LTX…
│   ├── Kernel factory ............................. GPU MODE ranks 1–8
│   └── Physics-gap method ......................... Decode at 72.8% of the physics limit
│
├── Systems, orchestration & ops: GPU fleets with automatic recovery.
│   ├── Spot-safe Linux GPU orchestrator ........... v1.12 → v4.1 in 3 months
│   ├── Linux and remote GPU operations ............ 5 GPU clouds run over SSH
│   ├── Baked container images and GPU templates ... Box start 24–39 s vs 423–460 s
│   └── Linux GPU orchestrators .................... Parallel runner: 3,551 lines, 4 isolated…
│
├── Factories & models: Pipelines specified as gated graphs.
│   └── Movie factory .............................. 193 gates, 4 clean audits
│
└── Agents & engineering frameworks: Agents with verifiable output.
    ├── Velocity & scale ........................... ~17,000 commits on GitHub
    └── Automated quality gates, proven… ........... 193 gates, each tested against 914 red and…
```

## Selected work

- [GPU kernels (GPU MODE)](https://github.com/Ganto12/portfolio/blob/main/systems/kernels.md): #1 in the world, prefix sum on H100
- [vLLM speed-up brief (results, not the recipe)](https://github.com/Ganto12/portfolio/blob/main/systems/vllm_brief.md): Stock vLLM → 1.86× batched
- [LeetGPU: 35/35 Hard kernels in a day](https://github.com/Ganto12/portfolio/blob/main/systems/leetgpu.md): 35/35 Hard + 4 Medium accepted
- [Quantization and compression: deploy, validate, measure](https://github.com/Ganto12/portfolio/blob/main/systems/quantization.md): FP8: 50.1 → 27.6 GiB, up to 3.08× decode
- [Spot-safe Linux GPU orchestrator (ComfyUI)](https://github.com/Ganto12/portfolio/blob/main/orchestration/spot_comfy_orc.md): v1.12 → v4.1 in 3 months
- [Linux and remote GPU operations](https://github.com/Ganto12/portfolio/blob/main/orchestration/linux_ops.md): 5 GPU clouds run over SSH
- [Movie factory: a film as a gated graph](https://github.com/Ganto12/portfolio/blob/main/factories/movie_factory.md): 193 gates, 4 clean audits

All 14 write-ups: [portfolio](https://github.com/Ganto12/portfolio).

Code lives in private repositories; access on request.
