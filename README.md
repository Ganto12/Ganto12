# AI systems that ship and scale

Self-taught in computer science and ML. Solo, with parallel agents, I build GPU kernels, inference, model training and a content engine that turns one brief into high-quality video and organic posts, plus consumer apps.

- **#1**: reached on the GPU MODE prefix-sum leaderboard, H100 (Oct 2026); top 10 on two more boards (#9 matmul, #9 vector sum); 35/35 Hard on LeetGPU
- **27×**: below MiniMax's own H3 API on text-to-video, per second of video, running MiniMax H3 on my own GPUs
- **1 engine**: brief → hook-first copy → factorial cells → locked pack → recipe → sub-factory render → gates → high-quality video and organic posts, every output traceable (11,764-clip batch runs)
- **30,000+ h**: in video games (Rainbow Six Siege) and the gym, reused as mental models to learn GPUs: the lexicon factory

Numbers are measured unless marked "about". Code lives in private repositories; full method, code walkthroughs and receipts are available in a working session.

## Map

```text
Write-ups
├── Content engine: One brief in, high-quality video and organic posts out.
│   ├── Organic content engine ..................... k^p cells, locked packs, every output traced
│   ├── Long-form video factory .................... 5 stages, 29 steps, 11,764-clip batches
│   ├── Movie factory .............................. 193 gates, 34-node graph, 4 clean audits
│   └── QuantEditor ................................ 121 creator videos; Markovian + goal-directed
│
├── GPU kernels: Hand-tuned kernels, ranked on public leaderboards.
│   ├── GPU kernels ................................ #1 GPU MODE (Oct 2026), #9 ×2, 35/35 LeetGPU…
│   └── Kernel debugging ........................... 46 of 50 drills diagnosed, 0 false confirms
│
├── Inference and training: Faster models and training on the same GPUs.
│   ├── vLLM speed-ups ............................. 3.08× single-stream, 1.86× at concurrency 32
│   ├── Physics-gap method ......................... Decode at 72.8% of speed-of-light; ~6 ms of…
│   ├── Video inference on my own GPUs ............. MiniMax H3 27× below its own API
│   └── Multi-GPU training and serving ............. 70B LoRA on 2× B200, served TP=2
│
├── How I learn: Mapping hard systems onto worlds I already know.
│   ├── Lexicon factory ............................ GPU as a gym: 114 tricks, 0 world leaks
│   └── Mental models & insights ................... 25+ models, 7 original insights
│
├── Agent harnesses: Harnesses that keep fleets of agents honest.
│   ├── Agent harnessing ........................... 280 skills; 12+ orchestrators → 4 overseers…
│   ├── Automated quality gates, tested like… ...... 71 executable gates, 0 mutation survivors
│   └── Self-auditing agents ....................... One report contract; weekly self-audit
│
├── MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.
│   ├── GPU fleet .................................. Up to 10 candidates a wave; nothing bills idle
│   ├── Rented-GPU job runners ..................... Idempotent: zero lost or repeated jobs
│   ├── Pinned runtimes and baked images ........... Box start 24–39 s vs 423–460 s
│   └── Infrastructure and security ................ 5 GPU clouds over SSH; 0 secrets in 129,503…
│
├── Backend: The plumbing that makes factories reusable.
│   ├── Factory registry ........................... One registry, 40+ registered factories
│   └── Decisions API gates ........................ ~$0.0002 a clip; AUC 0.90 vs 0.69 alone
│
└── Apps: Consumer products, shipped.
    ├── Onboarding and paywalls .................... Winning flow takes 100% of iOS onboarding
    ├── Voice SDR .................................. ~49 ms STT; 20, 200 and 800 ms reply paths
    └── iOS native apps ............................ 6 SwiftUI apps, 20 locales, 1 on the App Store
```

## Content engine: One brief in, high-quality video and organic posts out.

One engine: brief → factorial combo (k^p over named axes) → fixed recipe → gated render → finished long-form video or organic post. Every stage is gated and recorded as data, so every output traces back to its combo, and a new format is a new recipe or a new set of values, not new code.

| Write-up | Result |
|---|---|
| [Organic content engine: a k^p factorial design with every experiment traceable](engine/kp_content_engine.md) | k^p cells, locked packs, every output traced |
| [Long-form video factory: 5 gated stages, one database row per video](engine/ready_longs.md) | 5 stages, 29 steps, 11,764-clip batches |
| [Movie factory: a film as a gated graph](engine/movie_factory.md) | 193 gates, 34-node graph, 4 clean audits |
| [QuantEditor: Markovian and goal-directed video editing, learned from top creators](engine/quanteditor.md) | 121 creator videos; Markovian + goal-directed |

## GPU kernels: Hand-tuned kernels, ranked on public leaderboards.

Agents, physics hypotheses and kill-tests run as one loop; the leaderboard decides. Each mechanism is learned first through the lexicon factory.

| Write-up | Result |
|---|---|
| [GPU kernels: #1 on GPU MODE, the factory behind it, and 35/35 LeetGPU Hard](kernels/gpu_kernels.md) | #1 GPU MODE (Oct 2026), #9 ×2, 35/35 LeetGPU Hard |
| [Kernel debugging: a drill factory and the real incidents behind it](kernels/kernel_debugging.md) | 46 of 50 drills diagnosed, 0 false confirms |

## Inference and training: Faster models and training on the same GPUs.

Find the gap to the hardware limit, then pick validated levers: serving, quantization, speculative decoding and evals, each measured against a baseline.

| Write-up | Result |
|---|---|
| [vLLM speed-ups: FP8, FP8 KV cache and MTP, picked and validated](inference/vllm_speedups.md) | 3.08× single-stream, 1.86× at concurrency 32 |
| [Physics-gap method: where the time goes, then kill-test the cause](inference/physics_gap.md) | Decode at 72.8% of speed-of-light; ~6 ms of the MTP step sized |
| [Video inference on my own GPUs: MiniMax H3 at 27× below its own API](inference/video_inference_cost.md) | MiniMax H3 27× below its own API |
| [Multi-GPU training and serving: 70B fine-tunes on 2× B200, then letting a classifier win](inference/multi_gpu_training.md) | 70B LoRA on 2× B200, served TP=2 |

## How I learn: Mapping hard systems onto worlds I already know.

Self-taught in computer science and ML. I map each GPU and inference concept onto worlds where I have 30,000+ hours (video games, Rainbow Six Siege, the gym), lock the mapping only once it survives a real kernel or number, and feed it back into the kernel and inference work.

| Write-up | Result |
|---|---|
| [Lexicon factory: learning GPUs through worlds I already know, rendered as a 3D kernel world](learning/lexicon_factory.md) | GPU as a gym: 114 tricks, 0 world leaks |
| [Mental models & insights](learning/mental_models.md) | 25+ models, 7 original insights |

## Agent harnesses: Harnesses that keep fleets of agents honest.

Frameworks that make fleets of agents repeatable, auditable and fast.

| Write-up | Result |
|---|---|
| [Agent harnessing: skills, clean-context agents and sustained output](agents/harnessing.md) | 280 skills; 12+ orchestrators → 4 overseers + 11 launchers |
| [Automated quality gates, tested like code](agents/automated_gates.md) | 71 executable gates, 0 mutation survivors |
| [Self-auditing agents: one report contract and a weekly autopsy](agents/self_auditing_agents.md) | One report contract; weekly self-audit |

## MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.

Pinned images, one runtime per model mode, race launchers and overseers: rent, verify, run, recover and tear down without babysitting.

| Write-up | Result |
|---|---|
| [GPU fleet: buying agents that race the cheapest boxes](mlops/gpu_fleet.md) | Up to 10 candidates a wave; nothing bills idle |
| [Rented-GPU job runners: cheapest boxes, fully used, zero lost work](mlops/job_runners.md) | Idempotent: zero lost or repeated jobs |
| [Pinned runtimes and baked images](mlops/runtimes_and_images.md) | Box start 24–39 s vs 423–460 s |
| [Infrastructure and security: workstation, VM and GPU clouds](mlops/infrastructure.md) | 5 GPU clouds over SSH; 0 secrets in 129,503 files |

## Backend: The plumbing that makes factories reusable.

Databases, queues, scrapers and publishing routes that keep everything else moving.

| Write-up | Result |
|---|---|
| [Factory registry: every factory a Lego piece, every UI a game](backend/factory_registry.md) | One registry, 40+ registered factories |
| [Decisions API gates: fast yes/no checks with probabilities](backend/decisions_gates.md) | ~$0.0002 a clip; AUC 0.90 vs 0.69 alone |

## Apps: Consumer products, shipped.

From brand and design to paywall and App Store: things real people used and paid for.

| Write-up | Result |
|---|---|
| [Onboarding and paywalls: generated flows tested head to head](apps/onboarding_paywalls.md) | Winning flow takes 100% of iOS onboarding |
| [Voice SDR (full-duplex)](apps/voice_sdr.md) | ~49 ms STT; 20, 200 and 800 ms reply paths |
| [iOS native apps (SwiftUI)](apps/ios_native.md) | 6 SwiftUI apps, 20 locales, 1 on the App Store |
