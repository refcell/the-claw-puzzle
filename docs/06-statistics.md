# 06 – Statistics

## A 20-run rate is a weak signal
- **The binomial sd at p = 0.5, n = 20 is about 11 points.** With run-to-run correlation from sticky
  placements, it's closer to **13 points**.
- **The 95% interval around 60% is roughly 36–81%.** The top models on the board are statistically
  indistinguishable.
- **Pooled across many runs, the best players look alike:** 48% for one top competitor's models against about
  46% for our v2 family. Their 70% is a good 20-run window on a roughly 50% policy.

## The leaderboard is a max over models
Only each player's best model counts, and ties share rank. So running more models to 20 runs raises the chance
that one gets lucky.

| True win rate | P(≥ 14/20) | Models for a 50% chance at a tied #1 | P(≥ 15/20) | Models for a 50% chance at outright #1 |
|---|---|---|---|---|
| 0.50 | 5.8% | ~12 | 2.1% | ~34 |
| 0.55 | 13% | ~5 | 5.5% | ~13 |
| 0.60 | 25% | ~3 | 12.6% | ~6 |

At about $0.30–0.40 net per 20-run model, a portfolio is cheap. We call this out because it's how the board
works, not because it measures skill.

## State-adjusted scoring
To compare models across very different machine conditions:
- **Expected wins `E`** = the sum, over a model's runs, of the field's win rate at that run's loss streak (0..6+).
- **Report `O/E`** (observed over expected) and `O − E`.
- **Noise:** the sd of `O − E` over 20 runs is about 2 wins, so only `|O − E| ≥ 4` on 20 runs counts as strong.

A variant: compute the expectation from the outcomes of the ±60 neighbouring runs by *other* models
(predecessor-adjusted lift). It also removes time-of-day drift.

| Model | Runs | O − E | O/E |
|---|---|---|---|
| hp-fl1 `f67ecdca` | 71 | **+14.7** | 1.76 (z ≈ 4) |
| cnn-v2 `9758e28b` | 20 | +5.7 | 1.91 |
| hp-fg2 `8fb77333` | 208 | +1.4 | 1.03 (average) |

## Pairing, done right
- **Alternate order** (VC, CV, …), and report the results split by order.
- **Expect most pairs to be concordant.** Only discordant pairs carry information.
- **Use a control at the field average, or skip the control** and score each run against its predecessor's
  outcome instead: about 0.39 expected after a win, about 0.21 after a loss.
