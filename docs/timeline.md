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
