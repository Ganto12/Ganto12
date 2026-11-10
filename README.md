# AI systems that ship and scale

Self-taught in computer science and ML. I design systems. Solo, I run a company of artificial minds: agent harnesses are the management, factories are the departments, gates are quality control. With it I've built content factories, the GPU fleet and inference under them, kernels, and consumer apps. Model training is a small part.

- **#1** on the GPU MODE prefix-sum leaderboard (H100, Oct 2026), reached with the kernel factory; also #4 on B200 prefix sum at full precision (315.7 µs, 10 Nov 2026), #8 matmul (H100) and #9 vector sum (H100); 35/35 Hard on LeetGPU
- **27–250×** cheaper than frontier APIs at quality my eval gates accept: 27× vs MiniMax H3 text-to-video, 57.5–125× vs standard OpenAI image routes, 15–250× vs Seedance 2 ([write-up](inference/video_inference_cost.md))
- **1 engine**, end to end, over a Cartesian plane of hypotheses: hook-first copy, immutable packs, recipes, sub-factory renders and gates, with every output traceable (11,764-clip batch runs)

Numbers are measured unless marked "about". Code lives in private repositories; full method, code walkthroughs and receipts are available in a working session.

## How it fits together

It started with organic content systems on a bootstrapped budget. Midjourney video gave me about 24 videos a day, and only if I sat at the computer all day.

That bottleneck led me to open models, Wan 2.1 and then 2.2, on rented GPUs. Labs and inference providers were far too expensive for the volume I needed, so cost per output became an obsession.

Running the whole system as efficiently as possible forced me to understand workflows that do not work out of the box on every GPU, and each wall pushed me a layer down: the GPU fleet, then serving and quantization, then kernels.

## Map

```text
Write-ups
├── System design: Running a company of artificial minds.
│   ├── Agent harnessing ........................... 280 skills, 4 overseers, 11 launchers
│   ├── Automated gates, tested like code .......... 71 executable gates, 0 mutation survivors
│   └── Self-auditing agents ....................... One report contract; weekly self-audit
│
├── Mental models: General problem solving across domains, always in pursuit of mastery.
│   ├── Lexicon factory ............................ Locked words + 3D scenes
│   └── Mental models .............................. Games, sport, markets and GPUs
│
├── Content engine: One end-to-end engine over a Cartesian plane of hypotheses.
│   ├── Organic content engine ..................... k^p hypotheses, immutable packs
│   ├── Long-form video factory .................... 5 stages, 29 steps, 11,764-clip batches
│   ├── Movie factory .............................. 193 gates, 34-node graph, 4 clean audits
│   └── QuantEditor ................................ 121 creator videos; Markovian + goal-directed
│
├── Inference and kernels: Faster models on the same GPUs, down to hand-tuned kernels.
│   ├── Physics-gap method ......................... Measured vs theoretical limit, gap by gap
│   ├── vLLM speed-ups ............................. 3.08× single-stream, 1.86× at concurrency 32
│   ├── Video inference on my own GPUs ............. 27× cheaper than MiniMax's own H3 API
│   ├── Kernels
│   │   ├── Kernel factory ......................... GPU MODE #1, #4, #8, #9; 35/35 LeetGPU Hard
│   │   ├── Eval factory ........................... 4 verdicts, 23-case self-test
│   │   └── Kernel debugging ....................... 46 of 50 drills diagnosed, 0 false confirms
│   └── A little model training .................... 70B LoRA on 2× B200, served TP=2
│
├── MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.
│   ├── GPU fleet .................................. Up to 10 candidates a wave; nothing bills idle
│   ├── Rented-GPU job runners ..................... Idempotent: zero lost or repeated jobs
│   └── Pinned runtimes and baked images ........... Box start 24–39 s vs 423–460 s
│
├── Backend and infrastructure: Servers, databases, storage, security and the registry that ties factories together.
│   ├── Servers and databases ...................... 2 machines → 1 smaller VPS, same output
│   ├── Object storage ............................. Uploads verified before a job is marked done
│   ├── Security and cost watch .................... VPN-only; 0 secrets in 129,503 files
│   ├── Factory registry ........................... One registry, 40+ registered factories
│   └── Decisions API gates ........................ ~$0.0002 a clip; AUC 0.90 vs 0.69 alone
│
└── Apps: Consumer products, shipped.
    ├── Onboarding and paywalls .................... Winning flow takes 100% of iOS onboarding
    ├── Voice SDR .................................. ~49 ms STT; 20, 200 and 800 ms reply paths
    └── iOS native apps ............................ 6 SwiftUI apps, 20 locales, 1 on the App Store
```

## System design: Running a company of artificial minds.

The operating system behind everything below. Agent harnesses are the management layer: one skill per job, clean-context agents, one report contract and a weekly autopsy. Factories are the departments, each with one canonical way to run, registered in the [factory registry](backend/factory_registry.md). Gates are quality control, tested like code.

| Write-up | Result |
|---|---|
| [Agent harnessing: skills, clean-context agents and sustained output](agents/harnessing.md) | 280 skills, 4 overseers, 11 launchers |
| [Automated gates, tested like code](agents/automated_gates.md) | 71 executable gates, 0 mutation survivors |
| [Self-auditing agents: one report contract and a weekly autopsy](agents/self_auditing_agents.md) | One report contract; weekly self-audit |

## Mental models: General problem solving across domains, always in pursuit of mastery.

The lexicon factory turns any new concept into canonical words and 3D scenes from worlds I already know (30,000 to 68,000 hours across football, the gym and video games, Rainbow Six Siege above all), so it lands on old neural pathways; GPUs and inference came first. The mental models are how I attack problems in any domain.

| Write-up | Result |
|---|---|
| [Lexicon factory: canonical words and 3D scenes that translate any concept into worlds I already know](mental_models/lexicon_factory.md) | Locked words + 3D scenes |
| [Mental models: general problem solving across domains, in pursuit of mastery](mental_models/mental_models.md) | Games, sport, markets and GPUs |

## Content engine: One end-to-end engine over a Cartesian plane of hypotheses.

First-principles design of the most pertinent axes turns an open creative space into a Cartesian plane of hypotheses. Each cell ships as an immutable pack through recipes and sub-factories, with clean traceability from every output back to its cell; a new format is a new recipe, not new code.

| Write-up | Result |
|---|---|
| [Organic content engine: one end-to-end engine over a Cartesian plane of hypotheses](engine/kp_content_engine.md) | k^p hypotheses, immutable packs |
| [Long-form video factory: 5 gated stages, one database row per video](engine/ready_longs.md) | 5 stages, 29 steps, 11,764-clip batches |
| [Movie factory: a film as a gated graph](engine/movie_factory.md) | 193 gates, 34-node graph, 4 clean audits |
| [QuantEditor: Markovian and goal-directed video editing, learned from top creators](engine/quanteditor.md) | 121 creator videos; Markovian + goal-directed |

## Inference and kernels: Faster models on the same GPUs, down to hand-tuned kernels.

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
| [Kernel factory: #1, #4, #8 and #9 on GPU MODE boards, 35/35 LeetGPU Hard](kernels/kernel_factory.md) | GPU MODE #1, #4, #8, #9; 35/35 LeetGPU Hard |
| [Eval factory: is a faster path still the same model?](kernels/eval_factory.md) | 4 verdicts, 23-case self-test |
| [Kernel debugging: a drill factory and the real incidents behind it](kernels/kernel_debugging.md) | 46 of 50 drills diagnosed, 0 false confirms |

## MLOps and GPU fleet: Reproducible runtimes and GPU fleets that recover on their own.

Pinned images, one runtime per model mode, race launchers and overseers: rent, verify, run, recover and tear down without babysitting.

| Write-up | Result |
|---|---|
| [GPU fleet: buying agents that race the cheapest boxes, run over SSH](mlops/gpu_fleet.md) | Up to 10 candidates a wave; nothing bills idle |
| [Rented-GPU job runners: cheapest boxes, fully used, zero lost work](mlops/job_runners.md) | Idempotent: zero lost or repeated jobs |
| [Pinned runtimes and baked images](mlops/runtimes_and_images.md) | Box start 24–39 s vs 423–460 s |

## Backend and infrastructure: Servers, databases, storage, security and the registry that ties factories together.

One hardened VPS runs the agents, the queues and the database; GPU boxes write straight to B2 object storage; every machine is reachable only over a VPN, with watchers on every bill; and one database registry makes every factory reusable.

| Write-up | Result |
|---|---|
| [Servers and databases: one hardened VPS for agents, queues and the database](backend/servers_and_databases.md) | 2 machines → 1 smaller VPS, same output |
| [Object storage: GPU workers write straight to B2](backend/object_storage.md) | Uploads verified before a job is marked done |
| [Security and cost watch: VPN-only machines, watchers on every bill](backend/security_and_cost_watch.md) | VPN-only; 0 secrets in 129,503 files |
| [Factory registry: every factory a Lego piece, every UI a game](backend/factory_registry.md) | One registry, 40+ registered factories |
| [Decisions API gates: fast yes/no checks with probabilities](backend/decisions_gates.md) | ~$0.0002 a clip; AUC 0.90 vs 0.69 alone |

## Apps: Consumer products, shipped.

From brand and design to paywall and App Store: things real people used and paid for.

| Write-up | Result |
|---|---|
| [Onboarding and paywalls: generated flows tested head to head](apps/onboarding_paywalls.md) | Winning flow takes 100% of iOS onboarding |
| [Voice SDR (full-duplex)](apps/voice_sdr.md) | ~49 ms STT; 20, 200 and 800 ms reply paths |
| [iOS native apps (SwiftUI)](apps/ios_native.md) | 6 SwiftUI apps, 20 locales, 1 on the App Store |
