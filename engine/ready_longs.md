# Long-form video factory: 5 gated stages, one database row per video

Topic in, finished long-form video out: fact-check, script, a trained cut plan, scenes, voice, music and SFX, then a final edit, with every video a database row moving through 5 gated stages.

## Results

| Measured result |
|---|
| 5 stages and 29 mainline steps per video |
| batch runs of 11,764 clips |
| 12 structured video types |
| providers swappable per step |

## Method (in brief)

- Stages: fact-check → story and constraints → sections, impact beats, TTS and first pass → scenes, music, SFX, scene legal check, image-model with QA, a coverage gate and character patching → package, mix gates, upscale, video bridge, hook render and OTIO stitch.

---

Part of my [GitHub profile](../README.md), chapter: Content engine.
