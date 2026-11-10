# Lexicon factory: canonical words and 3D scenes that translate any concept into worlds I already know

A factory that converts a new concept or topic into the world models I built over 30,000 to 68,000 hours of chasing mastery (football, the gym, video games, Rainbow Six Siege above all). It produces two things: canonical words, a locked lexicon where each concept has one agreed name in those worlds, and canonical scenes, interactive 3D visualisations that act the concept out. New ideas land on old, well-worn neural pathways, so I learn them far faster. GPUs and inference were the first domain.

## What it makes

- **Canonical words.** One canon sheet, the single source of truth. Every concept gets one locked name in each of two worlds, a gym franchise and a Siege range. Examples: GPU = gym or base; SM = rack or firing lane; HBM = plate factory or armoury; KV cache = the customer's training log or the intel file; prefill = the mission briefing; decode = one rep (or shot) per round; warp = a line of 32 identical specialists moving on one call.
- **Canonical scenes.** Interactive 3D visualisations built only from locked words: a scene acts the concept out step by step, with live numbers where a real kernel or model exists. Today: the Kernel Lexicon, a 114-trick arsenal, the memory hierarchy, kernel replays and multi-GPU serving.
- **Together.** The word gives a concept a handle; the scene shows it moving. Reading the word recalls the scene, and the scene already sits on a pathway from thousands of hours in those worlds.

## How a concept goes through it

1. **Propose:** draft a mapping from the new concept into a known world. Proposals are kept apart from locked words.
2. **Test:** the mapping must survive a real kernel, number or mechanism. If the numbers break it, it is rewritten.
3. **Lock:** it joins the canon sheet; builders may only use locked words, and the sheet outranks any single page.
4. **Render:** the locked words drive the 3D scenes, so every scene speaks the same language.

## Results

| Measured result |
|---|
| **Lexicon factory:** 58 locked canon lines |
| **Lexicon factory:** all 114 kernel tricks told in 2 frames (873 strings, 0 world leaks) |
| **Kernel world:** A kernel book, a 114-trick arsenal, a lexicon and step-by-step kernel and serving scenes |

## How it works

### Lexicon factory

- The vague ask behind it: with no formal computer science training, how do you get your head around what is happening inside a GPU? I mapped it onto the two worlds where I have the most hours, the gym and a video game, so each new concept lands on an existing pathway. Stacked domains multiply rather than add (a Lollapalooza effect, the inverse of NVLink scaling): two known worlds gave me far more than double the grip on a concept that started out vague.
- Speed of uptake: with this method I got the conceptual picture of ML, the visual intuition of how the pieces fit, in about a week, on top of a year of deploying models in my own pipelines without fully understanding every part.
- Why the pools transfer: staying near the top of Siege across seasons and metas is a proxy for how trading alpha works (an edge appears, spreads, decays, and the next needs a new way of playing); football shows the same pattern in tactics (Guardiola's positional play, Mourinho's low block and counter). The same patterns recur across domains.
- Character creators: years of video-game character creators, where every face and body feature is a slider from 0 to 1 and the sliders combine, built an intuition for design spaces: many continuous dials, each with a range, all interacting. It maps straight onto full-factorial experiment design (every combination of factors and levels), onto a model's hidden dimensions as dials, and onto attention, which the lexicon already acts out as the character creator.
- The canon sheet is the single source of truth for how GPU and inference concepts map to two frames, a gym franchise and a Siege range. Examples: GPU = gym or base; SM = rack or firing lane; HBM = plate factory or armoury; KV cache = the customer's training log (the scroll) or the intel file; prefill = signing the whole customer up at once or the mission briefing; decode = one rep (or shot) per round; warp = a line of 32 identical specialists moving on one call.
- Factory rules: proposals are kept separate from locked words; a mapping is locked only after it survives a real kernel or number; builders must use locked words; the canon sheet outranks any single page. Wrong analogies were rewritten when the numbers broke them.
- Output: interactive 3D teaching pages (Kernel Lexicon, arsenal, memory hierarchy, multi-GPU) built from the canon.
- How it fed the self-taught inference side: each mechanism (memory hierarchy, warps, prefill, decode, KV cache, collectives) was learned through a mapping and then tested against measured results. The benchmarks (GPU MODE #1 in Oct 2026, vLLM 1.86×, LeetGPU 35/35 Hard) are measured separately and stand on their own.

### Kernel world

- Kernel Arsenal: every optimisation trick as a piece of kit, deduplicated into one catalogue.
- Kernel Book: the pages collected into one book, from memory hierarchy to multi-GPU serving.
- Kernel modes: my GPU MODE prefix-sum kernel on H100 and a matmul on B200, played step by step with live numbers.
- Serving scenes: a transformer round, a token factory, floor by floor through real model layouts, and serving one model across GPUs.
- The gym franchise: one 3D page for serving a model across GPUs. One GPU is a gym of 148 racks; at every rack, four lifters under one bar are the 4 tensor cores per SM. Plays DP, TP, PP and EP, the NCCL clock-in (all-reduce), hangs and stragglers, and the NVL72 to scale. A share link is ready to send.
- Access: the gym franchise has a share link; the other pages are private and shared on request.

## Limits

- Honest limit: the mapping is how I learn and teach, not a claim that it caused the results.

## What this covers

- **Lexicon factory: learning hard systems through worlds I already know (the GPU as a gym and a shooting range)**: I learn complex systems by mapping them onto pathways I already have: the whole inside of a GPU becomes a gym franchise and a shooting range, locked into a canon and rendered as the 3D kernel world. It is how a year of hands-on GPU work (training runs, serving, my own pipelines) turned into kernels that reached #1 in the world on GPU MODE (Oct 2026).
- **Kernel world: the inside of a GPU as interactive 3D worlds**: Interactive pages that show how GPU kernels and model serving work, built in my locked gym and armoury language.

---

Part of my [GitHub profile](../README.md), chapter: Mental models.
