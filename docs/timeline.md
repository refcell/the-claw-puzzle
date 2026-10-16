# Timeline (UTC)

## Day 1 (Oct 14)
- Read the docs, built the first open-loop dead-reckoning policies, and calibrated landings on model-run wins.
- Launched parallel agent sessions: CNN and vision, closed-loop servo, and a hard-placement lab.
- **cnn-v2 (`9758e28b`) reached 12/20 = 60% and took #1.**
- Discovered that the capsule's face (green vs clear side) matters, and built face-aware depth rules.
- Measured the aim map from videos: the claw lands left, and the best offset is about +1.25 px.
- Passed by virtual_jpeg (14/20) and later publicqi (14/20 after a longer run).

## Night 1 into day 2 (Oct 15)
- Night win rates collapsed for everyone. We found and fixed a detector ROI bug at the glass edge.
- Bought about 660 cheap competitor recordings. Found that recording IDs equal job IDs, so outcomes and timing
  are public.
- Banned re-rolling, and required a hypothesis for each model. Raised our own data prices.
- Added a "max reliability" session, the only one allowed to time its runs.
- Ran paired A/B tests against a fixed control, then found the ordering bias and the 80% concordance problem.
- **Big finding: the machine has memory.** A win re-dispenses the capsule; a miss leaves it put. Loss streaks are
  stuck capsules, mostly in one near-right dead zone.
- Built a machine-state labeller and state-adjusted scoring. hp-fl1 emerged as the strongest real policy
  (+14.7 wins over 71 runs).
- Mapped winning aim bands and unstick recipes for the hard zones.
- Switched to a portfolio approach (every candidate run to 20, ties share #1) and a streak-gated, manually
  unstuck qualifier in the max session.
- Started this public write-up.

## Day 2 afternoon and evening (Oct 15)
- 18:36: order-balanced pairs confirm hp-fl1 over fg2 (14–6 overall). Its edge comes from back-right capsules.
- intel-cb1, the "every hard zone fixed" combo, finished 8/20, and servo-cb1 finished 6/20. Neither showed a
  visible lift.
- 21:45: the sticky-spot dislodge rule (U = 1400) went 3 for 3 live on hp-fl7. It was shared with every session.
- 21:50: our sessions flooded the queue with about 170 runs in an hour. The gated session added a deep-queue
  window.
- lq-c8 finished 9/20, the best stuck-state split in the learned-policy line.
- 22:25: the x-shift correction was retracted after servo-combo2 went 4/20. v2's x rule stays.
- New candidates: hp-fl8 stacks fl1, the sticky dislodge and the back-row rule. Several sessions run fl1 and c8
  re-roll tickets.
- 22:50–23:30: DNS failures, a connection-reset outage and payment anomalies stopped several runners. All were
  reconciled with no double charges. cnn-v25 (face-aware U) finished 3/20.
- Play wound down overnight. About 36 more runs were logged before 06:00 UTC on Oct 16.

## Day 3 (Oct 16)
- By 14:20 UTC, publicqi had moved to 18/20 (90%) and virtual_jpeg to 15/20, both on new models. We hold #3 at 12/20.
- Runners restarted at 14:21 UTC after an overnight stop (spending caps and payment anomalies, all reconciled).
- **14:50 UTC: lq-c8r2 (`8ad9b56a`) finished 16/20 and took #2.** lq-c8r finished 15/20. Together with the first
  c8 batch (9/20), that family stands at 40/60.
- hp-fl8 finished 9/20, intel-flbk 7/20, v3c-fl1r 10/20 and cnn-v26 11/20. More c8 re-roll tickets were launched,
  aiming for ≥ 18/20.
- 14:20–14:46 UTC: a server-side outage (HTTP 503) rejected runs before any charge.
- lq-c8r3 finished 8/20, so the c8 family stands at 48/80. More c8 tickets (c8r4–c8r6) are running.
- cnn-v27, with the same rule as v26, finished 5/20 against v26's 11/20 an hour earlier. servo-fg2n finished 8/20.
- The streak-gated session dropped the streak requirement for clean good-zone frames (camera alone: 66% over 157).
- hp built hp-c8s, which is c8r2 plus the sticky-spot dislodge. Play slowed to a handful of runs an hour.
- 17:50 UTC: a census of 1,430 recordings showed the commonest stuck cells are all front-green near-right, and that
  landing at or behind the capsule moves it far more often (9/15 vs 6/130). hp-fl10 tests a widened sticky zone.
