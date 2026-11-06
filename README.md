# AI systems that ship and scale

Solo, with parallel agents, I build GPU inference, model training and content factories, plus consumer apps.

- **#1**: globally on the GPU MODE prefix-sum leaderboard, H100
- **1.86×**: stock vLLM batched throughput, same hardware
- **35/35**: LeetGPU Hard kernels accepted in a day
- **155bn**: tokens through coding agents: Codex 112bn + Claude Code 42.9bn

## Selected work

- [GPU kernels (GPU MODE)](https://github.com/Ganto12/portfolio/blob/main/systems/kernels.md): #1 globally, prefix sum on H100
- [vLLM speed-up brief (results, not the recipe)](https://github.com/Ganto12/portfolio/blob/main/systems/vllm_brief.md): Stock vLLM → 1.86× batched
- [LeetGPU: 35/35 Hard kernels in a day](https://github.com/Ganto12/portfolio/blob/main/systems/leetgpu.md): 35/35 Hard + 4 Medium accepted
- [Quantization and compression: deploy, validate, measure](https://github.com/Ganto12/portfolio/blob/main/systems/quantization.md): FP8: 50.1 → 27.6 GiB, up to 3.08× decode
- [Spot-safe Linux GPU orchestrator (ComfyUI)](https://github.com/Ganto12/portfolio/blob/main/orchestration/spot_comfy_orc.md): v1.12 → v4.1 in 3 months
- [Linux and remote GPU operations](https://github.com/Ganto12/portfolio/blob/main/orchestration/linux_ops.md): 5 GPU clouds run over SSH
- [Movie factory: a film as a gated graph](https://github.com/Ganto12/portfolio/blob/main/factories/movie_factory.md): 191 gates, 4 clean audits

All 14 write-ups: [portfolio](https://github.com/Ganto12/portfolio).

Code lives in private repositories; access on request.
