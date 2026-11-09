# Long-form video factory: 5 gated stages, one database row per video

Topic in, finished long-form video out: fact-check, script, a trained cut plan, scenes, voice, music and SFX, then a final edit, with every video a database row moving through 5 gated stages.

## Results

| Measured result |
|---|
| 5 stages and 29 mainline steps per video |
| batch runs of 11,764 clips |
| 12 structured video types |
| providers swappable per step |

## How it works

- Full end to end: topic → transcript → cut plan (QuantEditor) → exactly N scenes → images/video → TTS → music/SFX → final edit.
- Providers plug in per step: local GPU models or paid APIs, chosen automatically with a clear default and swapped without touching the rest.
- First principles: the cut plan is the spine, and scenes are generated to fit it, not the other way round.
- Batch runs of 11,764 clips.
- Stages: fact-check → story and constraints → sections, impact beats, TTS and first pass → scenes, music, SFX, scene legal check, Midjourney with QA, a coverage gate and character patching → package, mix gates, upscale, video bridge, hook render and OTIO stitch. 29 mainline steps.
- Video types: 12 structured strategies, including animal biography, founder/builder, ranks of X, listicle, explainer and cinematic documentary; seven use an intro plus five chapters. Length is set by word count, or by minutes at a set words-per-minute.
- Per-stage switches: scene legal check, graphics and infographic lanes, TTS provider, music backend (ACE-Step by default, Suno as an option), SFX on or off, and four QA paths from manual to full-auto.
- Database row as a state machine with a per-row folder scaffold; V2 is DB-first. Kimi hook injection, Qwen QA, a scene legal guard, Wan i2v, SeedVR upscale and an OTIO stitcher.

---

Part of my [GitHub profile](../README.md), chapter: Content engine.
