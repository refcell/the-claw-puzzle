# 05 – Failures

These are the honest misses, roughly grouped.

## Control approaches that lost
- **Closed-loop servoing on the claw** (several versions, cl2–cl4 and dr1): 0/4 or worse. Feedback through a
  10 Hz camera with no inference during holds is slower and noisier than dead reckoning.
- **Tracking the claw top to predict the landing:** sd 3.7 px, against about 1.5 px for exact holds.
- **A CNN localizer** (MAE about 0.3 px offline, robust to lighting augmentation). It was worse than the colour
  detector at night once the ROI was fixed, so it was never the primary detector.
- **Calibrating on manual runs:** human timings are too noisy.

## Grip ideas: all null
Most losses with good aim are **slips at lift or pushes at contact.** None of these changed that:
- claw sway phase at the drop, settle length (including +200 ms), drop hold (fixed server-side at 250 ms)
- landing position along the capsule's long axis
- approach direction and a final reverse "tap" (2/16, which mostly just shifted the landing point)
- U-first move order (−2 over 5 pairs)
- a "left-shift" idea: raw win rates favoured landing slightly left, but once capsule position and player were
  controlled for, the effect was nil
- post-drop moves: they're never applied

- **The x-shift correction.** A video measurement suggested shifting landings right (+0.75 px green,
  +1.25 px teal). servo-combo2 added it and went 4/20, with mid-field misses landing right. The finding was retracted.
- **Combining every hard-zone fix** (intel-cb1 8/20, servo-cb1 6/20). On paper both sat in the winning band for all
  three hard zones, but no lift was visible at n = 20. Most of intel-cb1's runs went into stuck states.
- **Removing the deeper mid-field aim** (lq-c7): 6/20, so the deeper aim stayed.

## Analysis tools that misled us
- **A learned offline evaluator** (kernel nearest neighbours over about 900 day recordings).
  - Spearman ρ against live win rate: 0.19 in-sample, 0.04 when leaving each model out, −0.35 when also leaving
    out ±20 minutes of time.
  - It ranked a 2/20 model at the top.
  - Its in-sample separation was placement leakage, so we never use it to pick models.
- **A nearly identical policy pair:** cnn-v2 and fg2 issue the same holds on mid-field frames, yet their
  state-adjusted scores differed by 0.9. At 20 runs, rankings are mostly noise.

## Methodology mistakes
- **Re-rolling and abandoning IDs early.** Re-uploading the same policy under fresh IDs and dropping each after a
  bad start burned money and taught nothing. It was banned for a while. It was later re-allowed only as a
  deliberate portfolio, with every batch finished.
- **Paired A/B testing with a fixed control:**
  - **The control always ran second.** A losing variant grab can push the capsule, so slot order isn't neutral.
  - **About 80% of pairs were concordant.** The same placement usually gives the same result, so a fix worth
    2–5 points needs hundreds of pairs.
  - **The control was itself below average** (fg2, about 26%), which made every variant look good.
- **Conclusions drawn during stuck stretches.** For example, a night-fix screen went 0/16, but 15 of those runs
  were into stuck capsules, so it said nothing about the fix.
- **A day/night flag that used an absolute timestamp,** so a full day of daytime runs was counted as night.

## Operational failures
- **We crowded the machine.** At one point about 90% of all runs were ours. Our timing experiments couldn't find
  idle windows, and prices rose.
- **We flooded the queue again** (late day two). In one hour our sessions paid for about 170 runs. Queue depth
  stayed at 5–10 and prices reached $0.093, which also blocked the streak-gated session, since it needs at most one
  job ahead.
- **Camera gating had low fidelity.** By the time a gated job ran, jobs queued ahead had often moved the capsule.
- **Agent sessions crashed** on oversized session logs (a single 450 KB line). They were replaced by fresh
  sessions that resumed from notes.
- **Payment scripts.** There were a few parsing bugs: a balance-floor check that misread its input, an empty
  model ID, and a paid job that wasn't recognised. Our rule of reconciling quotes before paying again meant none
  of them caused a double charge.
