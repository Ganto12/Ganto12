# AI systems that ship and scale

- **#1** on two GPU MODE leaderboards, reached with the kernel factory: prefix sum (H100, Oct 2026) and histogram (B200, 12 Nov 2026); also #4 on B200 prefix sum at full precision (315.7 µs, 10 Nov 2026), #8 matmul (H100, Oct 2026) and #9 vector sum (H100); 35/35 Hard on LeetGPU
- **27–250×** cheaper than frontier APIs for perceptually lossless tradeoffs: 27× vs MiniMax H3 text-to-video, 57.5–125× vs standard OpenAI image routes, 15–250× vs Seedance 2 ([write-up](inference/video_inference_cost.md))
- **1 engine**, end to end, over a Cartesian plane of hypotheses: hook-first copy, immutable packs, recipes, sub-factory renders and gates, with every output traceable (11,764-clip batch runs)

Self-taught. I design agent-run systems: harnesses, factories and gates.

Numbers are measured unless marked "about". Code lives in private repositories; full method, code walkthroughs and receipts are available in a working session.

## Map

```text
Write-ups
├── System design
│   ├── Agent harnessing ........................... 280 skills, 4 overseers, 11 launchers
│   ├── Automated gates, tested like code .......... 71 executable gates, 0 mutation survivors
│   ├── Self-auditing agents ....................... One report contract; weekly self-audit
│   └── Lexicon factory ............................ 4-step gate (propose, test, lock, render)…
│
├── Content engine
│   ├── Organic content engine ..................... k^p hypotheses, immutable packs
│   ├── Long-form video factory .................... 5 stages, 29 steps, 11,764-clip batches
│   ├── Movie factory .............................. 193 gates, 34-node graph, 4 clean audits
│   └── QuantEditor ................................ 121 creator videos; Markovian + goal-directed
│
├── Inference and kernels
│   ├── Physics-gap method ......................... Measured vs theoretical limit, gap by gap
│   ├── vLLM speed-ups ............................. 3.08× single-stream, 1.86× at concurrency 32
│   ├── Video inference on my own GPUs ............. 27× cheaper than MiniMax's own H3 API
│   ├── Kernels
│   │   ├── Kernel factory ......................... GPU MODE 2x #1, #4, #8, #9; 35/35 LeetGPU Hard
│   │   ├── Eval factory ........................... 4 verdicts, 23-case self-test
│   │   └── Kernel debugging ....................... 46 of 50 drills diagnosed, 0 false confirms
│   └── A little model training .................... 70B LoRA on 2× B200, served TP=2
│
├── MLOps and GPU fleet
│   ├── GPU fleet .................................. Up to 10 candidates a wave; nothing bills idle
│   ├── Rented-GPU job runners ..................... Idempotent: zero lost or repeated jobs
│   └── Pinned runtimes and baked images ........... Box start 24–39 s vs 423–460 s
│
├── Backend and infrastructure
│   ├── Servers and databases ...................... 2 machines → 1 smaller VPS, same output
│   ├── Object storage ............................. Uploads verified before a job is marked done
│   ├── Security and cost watch .................... VPN-only; 0 secrets in 129,503 files
│   ├── Factory registry ........................... One registry, 40+ registered factories
│   └── Decisions API gates ........................ ~$0.0002 a clip; AUC 0.90 vs 0.69 alone
│
└── Apps
    ├── Onboarding and paywalls .................... Winning flow takes 100% of iOS onboarding
    ├── Voice SDR .................................. ~49 ms STT; 20, 200 and 800 ms reply paths
    └── iOS native apps ............................ 6 SwiftUI apps, 20 locales, 1 on the App Store
```

## System design

Agent harnesses: one skill per job, clean-context agents, one report contract and a weekly autopsy. Factories: each with one canonical way to run, registered in the [factory registry](backend/factory_registry.md). Gates: tested like code.

| Write-up | Result |
|---|---|
| [Agent harnessing: skills, clean-context agents and sustained output](agents/harnessing.md) | 280 skills, 4 overseers, 11 launchers |
| [Automated gates, tested like code](agents/automated_gates.md) | 71 executable gates, 0 mutation survivors |
| [Self-auditing agents: one report contract and a weekly autopsy](agents/self_auditing_agents.md) | One report contract; weekly self-audit |
| [Lexicon factory: an agent harness that maps new technical concepts onto known domains, gated against real numbers](agents/lexicon_factory.md) | 4-step gate (propose, test, lock, render); 114-trick kernel arsenal in 3D |

## Content engine

First-principles design of the most pertinent axes turns an open creative space into a Cartesian plane of hypotheses. Each cell ships as an immutable pack through recipes and sub-factories, with clean traceability from every output back to its cell; a new format is a new recipe, not new code.

| Write-up | Result |
|---|---|
| [Organic content engine: one end-to-end engine over a Cartesian plane of hypotheses](engine/kp_content_engine.md) | k^p hypotheses, immutable packs |
| [Long-form video factory: 5 gated stages, one database row per video](engine/ready_longs.md) | 5 stages, 29 steps, 11,764-clip batches |
| [Movie factory: a film as a gated graph](engine/movie_factory.md) | 193 gates, 34-node graph, 4 clean audits |
| [QuantEditor: Markovian and goal-directed video editing, learned from top creators](engine/quanteditor.md) | 121 creator videos; Markovian + goal-directed |

## Inference and kernels

Find the gap to the theoretical limit, then close it: serving levers first (quantization, speculative decoding, caching), each measured against a baseline, then hand-tuned kernels when the levers run out. The kernel factory runs agents, physics hypotheses and kill-tests as one loop, and the leaderboard decides; the eval factory checks that every faster path is still the same model. Model training is the smallest part of this work.

| Write-up | Result |
|---|---|
| [Physics-gap method: compare against the theoretical limit, then close the gap](inference/physics_gap.md) | Measured vs theoretical limit, gap by gap |
| [vLLM speed-ups: FP8, FP8 KV cache and MTP, picked and validated](inference/vllm_speedups.md) | 3.08× single-stream, 1.86× at concurrency 32 |
| [Video inference on my own GPUs: MiniMax H3 at 27× less than its own API](inference/video_inference_cost.md) | 27× cheaper than MiniMax's own H3 API |
| [A little model training: 70B LoRA fine-tunes on 2× B200, then letting a classifier win](inference/multi_gpu_training.md) | 70B LoRA on 2× B200, served TP=2 |

### Kernels

| Write-up | Result |
|---|---|
| [Kernel factory: 2x #1, #4, #8 and #9 on GPU MODE boards, 35/35 LeetGPU Hard](kernels/kernel_factory.md) | GPU MODE 2x #1, #4, #8, #9; 35/35 LeetGPU Hard |
| [Eval factory: is a faster path still the same model?](kernels/eval_factory.md) | 4 verdicts, 23-case self-test |
| [Kernel debugging: a drill factory and the real incidents behind it](kernels/kernel_debugging.md) | 46 of 50 drills diagnosed, 0 false confirms |

## MLOps and GPU fleet

Pinned images, one runtime per model mode, race launchers and overseers: rent, verify, run, recover and tear down without babysitting.

| Write-up | Result |
|---|---|
| [GPU fleet: buying agents that race the cheapest boxes, run over SSH](mlops/gpu_fleet.md) | Up to 10 candidates a wave; nothing bills idle |
| [Rented-GPU job runners: cheapest boxes, fully used, zero lost work](mlops/job_runners.md) | Idempotent: zero lost or repeated jobs |
| [Pinned runtimes and baked images](mlops/runtimes_and_images.md) | Box start 24–39 s vs 423–460 s |

## Backend and infrastructure

One hardened VPS runs the agents, the queues and the database; GPU boxes write straight to B2 object storage; every machine is reachable only over a VPN, with watchers on every bill; and one database registry makes every factory reusable.

| Write-up | Result |
|---|---|
| [Servers and databases: one hardened VPS for agents, queues and the database](backend/servers_and_databases.md) | 2 machines → 1 smaller VPS, same output |
| [Object storage: GPU workers write straight to B2](backend/object_storage.md) | Uploads verified before a job is marked done |
| [Security and cost watch: VPN-only machines, watchers on every bill](backend/security_and_cost_watch.md) | VPN-only; 0 secrets in 129,503 files |
| [Factory registry: 40+ factories, each registered once and reused by key](backend/factory_registry.md) | One registry, 40+ registered factories |
| [Decisions API gates: fast yes/no checks with probabilities](backend/decisions_gates.md) | ~$0.0002 a clip; AUC 0.90 vs 0.69 alone |

## Apps

Shipped: 1 app live on the App Store, 6 SwiftUI apps, 20 locales.

| Write-up | Result |
|---|---|
| [Onboarding and paywalls: generated flows tested head to head](apps/onboarding_paywalls.md) | Winning flow takes 100% of iOS onboarding |
| [Voice SDR (full-duplex)](apps/voice_sdr.md) | ~49 ms STT; 20, 200 and 800 ms reply paths |
| [iOS native apps (SwiftUI)](apps/ios_native.md) | 6 SwiftUI apps, 20 locales, 1 on the App Store |
