# Decisions API gates: fast yes/no checks with probabilities

The Decisions API as a programmatic quality agent: yes/no questions about a picture or clip, a probability back, and thresholds that decide.

## Results

| Measured result |
|---|
| About $0.10 per million input tokens (about $0.0002 a clip) |
| one gate on every faceless picture, one on UGC clips |

## How it works

- What it is: OpenAI's Decisions API takes a picture and yes/no predicate questions and returns a probability for each, built for fast programmatic gates. It is a separate API from the chat and responses APIs, and I never swap those in.
- Measured: on 605 reviewed clips (32 dodgy) the combined score reaches AUC 0.90 against 0.69 for the model alone. An earlier 69-clip check had 4 of 5 dodgy clips in the top 17, and about 1 in 4 escalations was truly dodgy.
- Everything that can change lives in registry data (model, threshold, price, question wording, switches); the code only builds calls, keeps receipts and decides.

---

Part of my [GitHub profile](../README.md), chapter: Backend and infrastructure.
