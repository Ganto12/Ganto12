# AI systems that ship and scale

Solo, with parallel agents, I build GPU inference, model training and content factories, plus consumer apps.

- **#1**: reached on the GPU MODE prefix-sum leaderboard, H100 (Oct 2026)
- **800**: finished posts in about 9 days from one recipe-driven assembly line
- **27×**: below the model maker's own API on text-to-video, per second of video, on my own GPUs
- **162bn**: tokens through coding agents: Codex 119bn + Claude Code 42.9bn

## Map

```text
Portfolio
├── Factories: One brief in, thousands of traceable outputs.
│   ├── Recipe-driven assembly line ................ About 15 recipes, 800 posts in ~9 days
│   ├── Creative combos ............................ k^p combos, every output traceable
│   ├── Long-form video factory .................... 5 stages, 29 steps, 11,764-clip batches
│   ├── FactoryOps ................................. Every factory registered, findable and…
│   ├── MiniMax H3 video factory on my own GPUs .... 27× below MiniMax's own API
│   ├── Image-gen onboarding factory ............... Live in Superwall
│   ├── Movie factory .............................. 193 gates, 4 clean audits
│   ├── Decisions API gates ........................ About $0.10 per million input tokens
│   ├── Training our own model, then letting a… .... Fine-tuned a 70B, then a classifier matched it
│   ├── QuantEditor ................................ 121 training videos
│   └── Creative abstraction equations ............. k^p factorial design: axes × named levels
│
├── Agent harnesses: Harnesses that keep fleets of agents honest.
│   ├── Agent harnessing & frameworks .............. 280 skills, 110 of them factory-type
│   ├── Automated quality gates, proven… ........... 193 gates, each tested against 921 red and…
│   ├── System evolution ........................... 12+ orchestrator scripts → 4 overseers + 11…
│   ├── Agent reporting standard ................... One report contract across dozens of agents
│   ├── Weekly autopsy & agent debate factory ...... Agents audit and patch their own rules weekly
│   ├── Factory UI spec ............................ 8 rules, zero free-text inputs
│   └── Velocity & scale ........................... ~17,000 commits on GitHub
│
├── MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.
│   ├── GPU buying agents .......................... Cheapest of 5–10 boxes kept, the rest shut…
│   ├── Baked container images and GPU templates ... Box start 24–39 s vs 423–460 s
│   ├── MLOps container library .................... 11 Dockerfiles, ~10 image families
│   ├── One pinned runtime per model mode .......... One locked runtime per model mode, picked…
│   ├── Rented-GPU job runners ..................... Cheapest GPU that fits, kept busy, no idle…
│   ├── Cheap spot GPUs, zero lost work ............ Idempotent: zero lost or repeated jobs
│   ├── Vast.ai fleet .............................. 12+ launchers, 6 overseers, nothing bills idle
│   ├── Linux and remote GPU operations ............ 5 GPU clouds run over SSH
│   ├── B2 object storage .......................... Vast GPU workers write straight to B2
│   ├── Multi-machine production architecture ...... Workstation, VM and GPU clouds; every repo…
│   ├── Multi-GPU LLM serving and Kubernetes… ...... Simulated: second GPU cut Llama-8B…
│   ├── Cloud ops and security ..................... Hardened, VPN-only workstation
│   └── Doing more on less ......................... 2 machines to 1 smaller one, same output
│
├── Inference and kernels: Faster models on the same GPUs.
│   ├── Kernel factory ............................. Reached #1 on GPU MODE (Oct 2026)
│   ├── GPU kernels ................................ Reached #1, prefix sum on H100 (Oct 2026)
│   ├── Kernel debug factory ....................... 30 failure classes
│   ├── Debugging for real ......................... 8 real incidents
│   ├── Lexicon factory ............................ The GPU as a gym and a shooting range
│   ├── Kernel world ............................... A kernel book, a 114-trick arsenal, a…
│   ├── Physics-gap method ......................... 2 hidden bottlenecks: ~20% of each decode step
│   ├── vLLM speed-up brief ........................ Stock vLLM → 1.86× batched
│   ├── Quantization and compression ............... FP8: 50.1 → 27.6 GiB, up to 3.08× decode
│   ├── Local vs API cost arbitrage ................ Per 10 s of video on my own GPUs vs the…
│   ├── Self-hosted inference stack ................ Video generation 27× below MiniMax's own H3…
│   ├── Inference eval lab ......................... Stock vLLM → 1.86× batched throughput
│   ├── LLM fine-tuning & training pipeline ........ 7+ models
│   ├── ComfyUI creative pipelines &… .............. 60+ worker scripts
│   └── LeetGPU .................................... 35/35 Hard in a day
│
└── Apps and backend: Products and the plumbing under them.
    ├── Voice SDR .................................. About 49 ms speech-to-text
    ├── Programmatic paywalls ...................... 2 flows head to head; winner at 100% of iOS
    ├── iOS native apps ............................ 6 SwiftUI apps
    ├── Garden ..................................... 35 versions in one day
    └── NocoDB-backed ops subsystems ............... 4 subsystems
```

## Selected work

- [Recipe-driven assembly line: one fixed recipe per batch shape](https://github.com/Ganto12/portfolio/blob/main/factories/postqueue_prep.md): About 15 recipes, 800 posts in ~9 days
- [Creative combos: a k^p factorial design over named axes](https://github.com/Ganto12/portfolio/blob/main/factories/marketing_director.md): k^p combos, every output traceable
- [Long-form video factory: 5 gated stages, one database row per video](https://github.com/Ganto12/portfolio/blob/main/factories/ready_longs.md): 5 stages, 29 steps, 11,764-clip batches
- [Agent harnessing & frameworks](https://github.com/Ganto12/portfolio/blob/main/agents/harnessing.md): 280 skills, 110 of them factory-type
- [Automated quality gates, proven against test cases](https://github.com/Ganto12/portfolio/blob/main/agents/automated_gates.md): 193 gates, each tested against 921 red and 128 green test cases
- [System evolution: central orchestrator → clean-context agents](https://github.com/Ganto12/portfolio/blob/main/agents/system_evolution.md): 12+ orchestrator scripts → 4 overseers + 11 launchers
- [GPU buying agents: race the cheapest boxes, keep the first that works](https://github.com/Ganto12/portfolio/blob/main/orchestration/gpu_harness.md): Cheapest of 5–10 boxes kept, the rest shut down
- [Baked container images and GPU templates](https://github.com/Ganto12/portfolio/blob/main/orchestration/container_templates.md): Box start 24–39 s vs 423–460 s
- [MLOps container library](https://github.com/Ganto12/portfolio/blob/main/orchestration/mlops.md): 11 Dockerfiles, ~10 image families
- [Kernel factory: the system that reached #1 on GPU MODE](https://github.com/Ganto12/portfolio/blob/main/systems/kernel_factory.md): Reached #1 on GPU MODE (Oct 2026)
- [GPU kernels (GPU MODE)](https://github.com/Ganto12/portfolio/blob/main/systems/kernels.md): Reached #1, prefix sum on H100 (Oct 2026)
- [Kernel debug factory: 50 incidents, one canonical step graph](https://github.com/Ganto12/portfolio/blob/main/systems/debug_hub.md): 30 failure classes
- [Voice SDR (full-duplex)](https://github.com/Ganto12/portfolio/blob/main/apps/voice_sdr.md): About 49 ms speech-to-text
- [Programmatic paywalls: two iOS onboarding flows and a web-to-app funnel (Superwall)](https://github.com/Ganto12/portfolio/blob/main/apps/paywall_ab_flows.md): 2 flows head to head; winner at 100% of iOS
- [iOS native apps (SwiftUI)](https://github.com/Ganto12/portfolio/blob/main/apps/ios_native.md): 6 SwiftUI apps

All 51 write-ups: [portfolio](https://github.com/Ganto12/portfolio).

Code lives in private repositories; access on request.
