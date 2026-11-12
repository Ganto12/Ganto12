# Lexicon factory: an agent harness that maps new technical concepts onto known domains, gated against real numbers

An agent harness that maps a new technical concept onto domains I already know and produces two things: locked canonical words (one name per concept) and canonical 3D scenes that act the concept out. Every mapping passes a 4-step gate (propose, test against a real kernel or number, lock, render). GPUs and inference were the first domain.

## What it makes

- **Canonical words.** One canon sheet, the single source of truth. Every concept gets one locked name in each of two worlds, a gym franchise and a Siege range. Examples: GPU = gym or base; SM = rack or firing lane; HBM = plate factory or armoury; KV cache = the customer's training log or the intel file; prefill = the mission briefing; decode = one rep (or shot) per round; warp = a line of 32 identical specialists moving on one call.
- **Canonical scenes.** Interactive 3D visualisations built only from locked words: a scene acts the concept out step by step, with live numbers where a real kernel or model exists. Today: the Kernel Lexicon, a 114-trick arsenal, the memory hierarchy, kernel replays and multi-GPU serving.
- **Together.** The word gives a concept a handle; the scene shows it moving. Reading the word recalls the scene.

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

- The canon sheet is the single source of truth for how GPU and inference concepts map to two frames, a gym franchise and a Siege range. Examples: GPU = gym or base; SM = rack or firing lane; HBM = plate factory or armoury; KV cache = the customer's training log (the scroll) or the intel file; prefill = signing the whole customer up at once or the mission briefing; decode = one rep (or shot) per round; warp = a line of 32 identical specialists moving on one call.
- Factory rules: proposals are kept separate from locked words; a mapping is locked only after it survives a real kernel or number; builders must use locked words; the canon sheet outranks any single page. Wrong analogies were rewritten when the numbers broke them.
- Output: interactive 3D teaching pages (Kernel Lexicon, arsenal, memory hierarchy, multi-GPU) built from the canon.

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

- **Lexicon factory: mapping GPU concepts onto a gym franchise and a shooting range**: The whole inside of a GPU becomes a gym franchise and a shooting range, locked into a canon and rendered as the 3D kernel world. It is how a year of hands-on GPU work (training runs, serving, my own pipelines) turned into kernels with two #1s on GPU MODE (Oct and Nov 2026).
- **Kernel world: the inside of a GPU as interactive 3D worlds**: Interactive pages that show how GPU kernels and model serving work, built in my locked gym and armoury language.

---

Part of my [GitHub profile](../README.md), chapter: System design.
