# Weight-space programming: fixing a video LoRA by editing its weights

MiniMax H3's Turbo-8 speed LoRA kept adding unwanted speech to silent clips. An LLM agent, working directly over the LoRA's weights with no gradients and no training, found the direction to cut; the edit built on it fixes most of the problem at no extra inference cost.

## Results

| Measured result |
|---|
| Unwanted speech cut 4.3× |
| clearly talking clips 9× rarer (9 of 30 → 1 of 30 on held-out prompts) |
| holds on 55 fresh seeds (p = 6e-11) |
| **cost:** 35% less motion. Same lane 1.58× faster with 8-bit attention |

## How it works

- What we tested: Turbo-8 samples MiniMax H3 (a joint audio and video DiT) in 8 steps instead of 20, about 2× faster, but it often adds a voice to prompts that ask for a silent reaction. A production scan flagged 538 of 1,586 Turbo-8 clips (34%).
- Frontier method: Sparks of Weight-Space Programming (Takehi, Teoh, Zhang and Khattab, Nov 2026) showed an LLM agent can program a model by editing its weights directly. I applied its Exp 8 setup to a production audio-video model within weeks of publication: the agent gets a Python kernel over the LoRA weights and a 25-render budget, may read weights, stage edits and render clips, but never take gradients.
- What the agent found: the LoRA's change is nearly rank 1, and removing the top singular direction in the late blocks' attention and MLP silences the speech. Its own narrow pick (E5) cut speech 1.6×.
- The shipped edit (S4) applies the agent's direction more widely: 120 modules in blocks 20 to 49, about 63.5% of the LoRA's weight change, picked on a separate dev set. The base model is never touched and the edited LoRA loads like the original.
- Results on 30 held-out silent prompts, same seeds: clearly talking clips 9 of 30 → 1 of 30 (9× fewer); unwanted speech 0.93 s → 0.21 s per clip (4.3× less); 10 of 10 scripted lines still spoken. On 55 fresh seeds voiced clips fell from 50 to 15, none started talking (sign test p = 6e-11).
- Honest cost: speech and motion ride the same direction, so S4 keeps 0.65 of the motion (35% less). A 6-group throttle meant to spare motion failed its pre-registered bar. S4 has no size-matched random control yet.
- Speed: attention is about 67% of each Turbo-8 step at 22,507 tokens, so ComfyUI's built-in 8-bit attention took the lane from 58.6 s to 37.1 s per 3 s clip (1.58× faster, 37% cheaper). S4 stacks on it at no speed cost.
- Status: early findings; two follow-up experiments running. Full write-up with side-by-side clips: https://github.com/Ganto12/h3-turbo-lora-weight-space-edit

---

Part of my [GitHub profile](../README.md), chapter: Inference and kernels.
