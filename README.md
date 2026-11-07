# AI systems that ship and scale

Solo, with parallel agents, I build GPU inference, model training and content factories, plus consumer apps.

- **#1**: in the world on the GPU MODE prefix-sum leaderboard, H100
- **800**: finished posts in about 9 days from one recipe-driven assembly line
- **$0.008**: per 3 s generated video clip on my own GPUs, 5×–10× below the model maker's API
- **162bn**: tokens through coding agents: Codex 119bn + Claude Code 42.9bn

## Map

```text
Portfolio
├── Factories: One brief in, thousands of traceable outputs.
│   ├── Recipe-driven assembly line ................ About 15 recipes, 800 posts in ~9 days
│   ├── Creative matrix ............................ Hook × audience × angle, every output…
│   ├── Long-form video factory .................... 5 stages, 29 steps, 11,764-clip batches
│   ├── MiniMax H3 video factory on my own GPUs .... About $0.008 a clip, 5×–10× below the API
│   ├── Movie factory .............................. 193 gates, 4 clean audits
│   ├── Decisions API gates ........................ About $0.10 per million input tokens
│   ├── QuantEditor ................................ 121 training videos
│   ├── Image-gen onboarding factory ............... Live in Superwall
│   └── Creative abstraction equations ............. k^p factorial design: axes × named levels
│
├── Agent harnesses: Harnesses that keep fleets of agents honest.
│   ├── Agent harnessing & frameworks .............. 280 skills, 110 of them factory-type
│   ├── Automated quality gates, proven… ........... 193 gates, each tested against 914 red and…
│   ├── System evolution ........................... 12+ orchestrator scripts → 4 overseers + 11…
│   ├── Agent reporting standard ................... One report contract across dozens of agents
│   ├── Weekly autopsy & agent debate factory ...... Agents audit and patch their own rules weekly
│   ├── Codex mission scoring ...................... Every agent mission scored, daily and weekly
│   ├── Factory UI spec ............................ 8 rules, zero free-text inputs
│   └── Velocity & scale ........................... ~17,000 commits on GitHub
│
├── MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.
│   ├── GPU fleet agent harness .................... Up to 10 live candidates per wave
│   ├── Baked container images and GPU templates ... Box start 24–39 s vs 423–460 s
│   ├── MLOps container library .................... 11 Dockerfiles, ~10 image families
│   ├── One pinned runtime per model mode .......... One locked runtime per model mode, picked…
│   ├── Linux GPU orchestrators .................... Rent, verify, run, retry, upload in one runner
│   ├── Spot-safe Linux GPU orchestrator ........... Interruptible GPUs, zero lost work
│   ├── Vast.ai fleet & GPU operations ............. 12+ launchers, 6 overseers, nothing bills idle
│   ├── Linux and remote GPU operations ............ 5 GPU clouds run over SSH
│   ├── B2 object storage .......................... Vast GPU workers write straight to B2
│   ├── Multi-machine production architecture ...... Workstation, VM and GPU clouds; every repo…
│   ├── Multi-GPU LLM serving and Kubernetes… ...... Simulated: second GPU cut Llama-8B…
│   ├── Cloud ops and security ..................... Hardened, VPN-only workstation
│   └── Doing more on less ......................... 2 machines to 1 smaller one, same output
│
├── Inference and kernels: Faster models on the same GPUs.
│   ├── GPU kernels ................................ #1 in the world, prefix sum on H100
│   ├── Kernel factory ............................. GPU MODE ranks 1–8
│   ├── Physics-gap method ......................... 2 hidden bottlenecks: ~20% of each decode step
│   ├── vLLM speed-up brief ........................ Stock vLLM → 1.86× batched
│   ├── Quantization and compression ............... FP8: 50.1 → 27.6 GiB, up to 3.08× decode
│   ├── Local vs API cost arbitrage ................ Video: 116×–250× below API cost (local LTX…
│   ├── Self-hosted inference stack ................ Video generation 116×–250× cheaper than…
│   ├── Inference eval lab ......................... Stock vLLM → 1.86× batched throughput
│   ├── LLM fine-tuning & training pipeline ........ 7+ models
│   ├── ComfyUI creative pipelines &… .............. 60+ worker scripts
│   ├── Kernel world ............................... A kernel book, a 114-trick arsenal, a…
│   └── LeetGPU .................................... 35/35 Hard + 4 Medium accepted
│
└── Apps and backend: Products and the plumbing under them.
    ├── Voice SDR .................................. About 49 ms speech-to-text
    ├── Programmatic paywalls and A/B test flows ... 107 paywall and flow builds
    ├── iOS native apps ............................ 6 SwiftUI apps
    ├── Garden ..................................... 35 versions in one day
    └── NocoDB-backed ops subsystems ............... 4 subsystems
```

## Selected work

- [Recipe-driven assembly line: one fixed recipe per batch shape](https://github.com/Ganto12/portfolio/blob/main/factories/postqueue_prep.md): About 15 recipes, 800 posts in ~9 days
- [Creative matrix: a fuzzy brief turned into a finite grid of reproducible cells](https://github.com/Ganto12/portfolio/blob/main/factories/marketing_director.md): Hook × audience × angle, every output traceable
- [Long-form video factory: 5 gated stages, one database row per video](https://github.com/Ganto12/portfolio/blob/main/factories/ready_longs.md): 5 stages, 29 steps, 11,764-clip batches
- [Agent harnessing & frameworks](https://github.com/Ganto12/portfolio/blob/main/agents/harnessing.md): 280 skills, 110 of them factory-type
- [Automated quality gates, proven against test cases](https://github.com/Ganto12/portfolio/blob/main/agents/automated_gates.md): 193 gates, each tested against 914 red and 127 green test cases
- [System evolution: central orchestrator → clean-context agents](https://github.com/Ganto12/portfolio/blob/main/agents/system_evolution.md): 12+ orchestrator scripts → 4 overseers + 11 launchers
- [GPU fleet agent harness (acquisition + overseers)](https://github.com/Ganto12/portfolio/blob/main/orchestration/gpu_harness.md): Up to 10 live candidates per wave
- [Baked container images and GPU templates](https://github.com/Ganto12/portfolio/blob/main/orchestration/container_templates.md): Box start 24–39 s vs 423–460 s
- [MLOps container library](https://github.com/Ganto12/portfolio/blob/main/orchestration/mlops.md): 11 Dockerfiles, ~10 image families
- [GPU kernels (GPU MODE)](https://github.com/Ganto12/portfolio/blob/main/systems/kernels.md): #1 in the world, prefix sum on H100
- [Kernel factory](https://github.com/Ganto12/portfolio/blob/main/systems/kernel_factory.md): GPU MODE ranks 1–8
- [Physics-gap method: where the time goes, then kill-test the cause](https://github.com/Ganto12/portfolio/blob/main/systems/physics_gap.md): 2 hidden bottlenecks: ~20% of each decode step
- [Voice SDR (full-duplex)](https://github.com/Ganto12/portfolio/blob/main/apps/voice_sdr.md): About 49 ms speech-to-text
- [Programmatic paywalls and A/B test flows (Superwall)](https://github.com/Ganto12/portfolio/blob/main/apps/paywall_ab_flows.md): 107 paywall and flow builds
- [iOS native apps (SwiftUI)](https://github.com/Ganto12/portfolio/blob/main/apps/ios_native.md): 6 SwiftUI apps

All 47 write-ups: [portfolio](https://github.com/Ganto12/portfolio).

Code lives in private repositories; access on request.
