# Onboarding and paywalls: generated flows tested head to head

An image-generation factory that reverse-engineers an onboarding flow and regenerates every screen, and the programmatic paywalls that run the flows head to head.

## Results

| Measured result |
|---|
| 2 main iOS onboarding flows head to head, plus in-app against web-to-app checkout |
| the winner, Flow D V9, takes 100% of iOS onboarding |
| **Live in Superwall:** Flow D V9 takes 100% of iOS onboarding traffic |
| Flow F is a full alternative art direction (12 pages) |

## Method (in brief)

- How tests run: each flow is a variant with a traffic weight in a Superwall campaign, so a new flow, price or offer ships to a share of users without an App Store release.

## Limits

- Honest limits: no split is running right now (each live audience has one variant at 100%), and conversion results for each variant aren't recorded in this hub.

## What this covers

- **Programmatic paywalls: two iOS onboarding flows and a web-to-app funnel (Superwall)**: Onboarding that ends in a live, programmatic paywall, with flows and prices tested from Superwall without an app release.
- **Image-gen onboarding factory: reverse-engineer a flow, regenerate every screen, map the hit areas**: Each onboarding screen, buttons and all, is one generated image; an agent then maps every tappable area so it works on every iPhone.

---

Part of my [GitHub profile](../README.md), chapter: Apps.
