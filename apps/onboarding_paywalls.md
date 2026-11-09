# Onboarding and paywalls: generated flows tested head to head

An image-generation factory that reverse-engineers an onboarding flow and regenerates every screen, and the programmatic paywalls that run the flows head to head.

## Results

| Measured result |
|---|
| **Programmatic paywalls:** 2 main iOS onboarding flows head to head, plus in-app against web-to-app checkout |
| **Programmatic paywalls:** the winner, Flow D V9, takes 100% of iOS onboarding |
| **Live in Superwall:** Flow D V9 takes 100% of iOS onboarding traffic |
| **Image-gen onboarding factory:** Flow F is a full alternative art direction (12 pages) |

## How it works

### Programmatic paywalls

- Programmatic paywall: the final page is not a picture. A looping video plays behind a fade, and the price card on top is live: plan names, tokens, the per-week and per-day price lines and the 'Claim 50% Off' button come from the store products at runtime, so prices and offers change without new art.
- Gender routing: the male and female routes each get their own paywall and video (pages 23 and 24 of Flow D V9), chosen from the onboarding answers.
- How tests run: each flow is a variant with a traffic weight in a Superwall campaign, so a new flow, price or offer ships to a share of users without an App Store release.
- What was tested: the two main onboarding flows head to head; iterations of the winner (video sliders, a daily price, a $0.00 trial, two-plan against three-plan membership); and in-app against web-to-app checkout with a 50% annual offer.
- Today: iOS sends 100% of onboarding to Flow D V9, with male and female paywalls on the web-to-app checkout.
- Why it matters: a new art direction or price test is a new variant and a traffic weight, not an App Store release.

### Image-gen onboarding factory

- Reverse-engineering first: top apps' onboarding flows are broken down screen by screen into a page map (what each screen asks, shows and sells), then the image model redraws every page in our own art direction.
- The idea: most teams build onboarding screen by screen in code or a design tool, because placing real buttons over custom art and keeping the hit areas right on every device is fiddly. Here the image model draws the whole screen in one shot, art, copy and buttons included, and an agent does the fiddly part.
- Inventory: the agent reads each screen and lists everything on it: back and close marks, the main button, each answer card, progress markers and empty space.
- Control rules: a clean baked button gets an invisible hit area exactly over it; a wrong or missing one is covered with a brand-matched control; answer cards each get their own hit area, and a tap selects and moves on. Nested art, labels and radio marks inside one answer merge into a single hit area, so there are no false targets.
- Device-proof geometry: every hit area is stored as a percentage of the image, not in one phone's pixels, kept at least 44 points on the smallest phone and clear of the notch, Dynamic Island and home bar, so the same map works on every iPhone size.
- Per-screen registry: one entry per screen with its source image, control types, hit-area box, expected event, haptic and animation, plus results for each iPhone preset, the coder, an independent auditor, before and after evidence, and publish state.
- Why it matters: a full new art direction (passport, classic passport, hair immigration) is a prompt pass and an agent mapping pass, not a rebuild, so the funnel can be restyled and tested quickly.

## Limits

- Honest limits: no split is running right now (each live audience has one variant at 100%), and conversion results for each variant aren't recorded in this hub.

## What this covers

- **Programmatic paywalls: two iOS onboarding flows and a web-to-app funnel (Superwall)**: Onboarding that ends in a live, programmatic paywall, with flows and prices tested from Superwall without an app release.
- **Image-gen onboarding factory: reverse-engineer a flow, regenerate every screen, map the hit areas**: Each onboarding screen, buttons and all, is one generated image; an agent then maps every tappable area so it works on every iPhone.

---

Part of my [GitHub profile](../README.md), chapter: Apps.
