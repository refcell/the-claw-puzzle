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

## Re-roll tickets and selection
The board counts each player's best model, so running several byte-distinct copies of one policy and keeping
the best is a legitimate strategy. Its best batch overstates the policy's true rate, though.
- **lq-c8 over 27 identical copies: 265/540 = 49%.** Copies ranged from 3/20 to 16/20. Our 16/20 board entry is its best
  draw, so the board overstates the policy by about 30 points. At a true 50%, P(≥ 14/20) is about 6% per copy, and P(≥ 18/20) about 0.02%.
- **A lighting effect that was really selection bias.** An early note claimed c8 was sensitive to light. It counted only
  the early copies. With all IDs, day is 53% (fresh 43/79) and dusk 47% (fresh 42/79), within noise of each other.
- **Copies of one policy scatter widely.** Nine hp-fl1 copies went 10, 10, 10, 8, 7, 13, 7, 8 and 8 out of 20 (81/180 = 45%). Four hp-fl11 copies total 42/100. Two hp-fl8
  copies went 9 and 7 (16/40). Every hard-placement build since fl8 (seven of them) has landed between 25% and 50%. The v26 rule went 11, 5, 9 and 7 out of 20 (32/80), and the v29 rule 19/60 over three batches. Later re-rolls of
  the two rules ranged from 3/20 to 12/20.
- **Fresh vs stuck dominates:** an hp-fl1 copy went 10/20 raw, but 6/7 from fresh states and 4/13 from stuck ones.
- **The same rule, an hour apart:** cnn-v26 and cnn-v27 have identical aim and went 11/20 and 5/20. A 20-run batch
  mostly measures its time window.
- If the true rate were 50%, P(≥ 16/20) in a single batch would be about 0.6%. At 67% it's roughly 15–20%, and with
  several tickets in flight a 16 is unsurprising.
- Time of day and machine state still dominate. The weak batch ran when the field was winning about 25%.

## Day vs night for the same policy
- servo's c8 copies went 46/80 by day and 16/40 at night (p ≈ 0.08). One proposed cause was the detector's teal flag
  misfiring at night. That was rejected: on the frame-10 capsule mask the flag disagreed on 0/40 night runs and 1/80 day
  runs. The gap is chance or contact physics.
- On one night the field won 70/183 = 38%, no worse than by day. A night-corrected fl1 build (v3c-fl1n) went 7/20 in that
  window, so its x correction showed no edge.
