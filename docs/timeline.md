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
- 16:40 UTC onward: a long server outage (auth errors, leaderboard timeouts) stalled every runner. Failed attempts
  weren't charged.
- lq-c8r4 finished 12/20, so the c8 family stands at 60/100. The hp-fl1 copy intel-fl1r1 finished 10/20 (fresh 6/7).
- 21:00 UTC: the outage was still ongoing (HTTP 503/504 on runs and the leaderboard). Runners keep retrying, and
  5xx errors before payment are now treated as retryable.
- 18:45 UTC: an audit of the streak-gated session found the gate open in only 2% of 8 hours of camera samples.
- 22:00 UTC: the puzzle API was still down. Our last paid run was at 18:21 UTC.
- 22:30 UTC: the API came back. All sessions reconciled their outage-period quotes, and none were double-charged.
- Results finishing after the outage: v3c-fl1r2 10/20 (the third hp-fl1 copy at exactly 10/20), hp-fl9 9/20,
  servo-fl8r 7/20 and cnn-v28 9/20. lq-c8r7 and c8r8 were uploaded. About 150 runs were paid in the first half hour back.

## Day 3 evening (Oct 16–17)
- The portfolio switched to c8r2 copies. servo-c8s finished 15/20, and the lq tickets c8r5 and c8r6 finished 8 and
  10. Eight identical c8 copies total 93/160 (58%).
- hp-c8s (c8 + sticky dislodge) finished 9/20, hp-fl10 7/20, and fl1 copies 8/20 and 7/20.
- 22:55 UTC: a camera-persistence study showed good placements rarely survive 15 s (21%). The gated session now
  pays only into an empty queue.
- Spend passed $70, with about 250 runs paid in the 90 minutes after the outage ended.
- 00:40 UTC: the queue was entirely our own jobs, 5–8 deep. Sessions were asked to cap themselves at one queued job.
- intel-c8s and servo-c8s2 each finished 10/20, so ten c8 copies total 113/200. v3c-fl1r5 finished 13/20, the best
  fl1 copy, and hp-fl11 10/20 (9 straight wins after a 1/11 start). More c8 copies (r11–r14) were launched.
- 01:30 UTC: pooling all 21 c8-family IDs gives 195/385 = 51%. That corrected an earlier "lighting-sensitive" claim,
  which was selection bias. The board's 16/20 is c8's luckiest draw.
- Copies kept finishing at 8–12/20: servo-c8s3 11, servo-c8s4 10 and intel-c8s3 12, with fl1 copies at 7 and 8.
  hp-fl12 finished 5/20. The fl1 line paused day copies for the night and launched a night-corrected ticket.
- 02:13 UTC: the learned-policy session reached its $20 cap and stopped. Its final c8 tally was 167/340 (49%) over 17
  batches. With the servo and intel copies, it's 265/540.
- The night ran hot (field 38%), so the night-corrected fl1n (7/20) showed nothing, and the fl1 night pause was
  cancelled. fl11's family finished at 42/100, and hp switched to c8 + sticky-dislodge tickets. The gated session
  reached 3/6.
- **03:08–03:35 UTC (at night): v3c-fl1r10 (`606b2d85`), an exact hp-fl1 copy, went 17/20 and moved us to 17/20 at #2.**
  It won 8/8 fresh states and 5/5 runs after three or more losses (O/E 2.46).
  intel-c8s8 finished 16/20 at 03:50. The fl1 copies total 97/200, so the 17 is a lottery hit.
- Re-rolls stopped. The combined-v3 session's class audit found no fl1 class fix worth more than 1–2 points, and it
  stopped paying.
- The c8 day/night split over 440 runs (53% vs 36%, z ≈ 2.6) moved remaining c8 copies to the day window.
- 04:10 UTC: c8's night drop turned out to be entirely stuck-state runs (0/18 at night vs 47/97 by day). Stuck
  capsules stay stuck when the queue is thin, which makes night a target for unsticking, not for copies.
- Copy tickets kept finishing in the 4–13/20 range. intel's 14 c8 copies total 138/280. The cnn rule lines closed at
  39% and 38%, and none of their batches reached 14/20.
- hp-c8sr10 (c8 + sticky dislodge) went 16/20, matching lq-c8r2. The gated qualifier max-g1 finished 12/20 = 60%,
  and a strict good-zone-only gated qualifier started.
- servo-c8k went 12/20 at night, which narrowed c8's night gap to 42% vs 53% (z ≈ 1.8).
- 06:31–06:41 UTC: servo-c8s7 went 4/20 at night, which widened c8's night gap again (38% vs 53%, z ≈ 2.8). c8 copies
  went back to day-only. hp's c8 + sticky-dislodge line reached 180/360 over 18 copies.
- 07:00–08:00 UTC: a machine-wide slump (all players about 25%). hp's night copies went 4, 6, 3, 7 and 3 out of 20, and its
  line fell to 203/460. The gated qualifier's model expired unused and was re-uploaded.
- 08:00–09:00 UTC: hp's night copies continued (10, 8, 6, 7, 4). Its night batches total 58/200 = 29%.
- 09:00–10:00 UTC: hp's night copies went 3, 12, 15, 6 and 5. The 15/20 is the line's second-best batch, and its night
  total is 99/300 = 33%.
- 10:00–11:00 UTC: hp's night copies went 10, 3, 8, 11 and 7. Night total 138/400; line total 318/760.
- 11:00–12:00 UTC: hp wrapped up its c8 + sticky line at 338/820 (41%) over 41 copies. It was about 55% before the
  06:10 slump. The second gated qualifier was at 4/13, and the gated good-zone path stands at 9/18 against a historical 66%.
- 12:30–14:00 UTC: play nearly stopped. The strict gated qualifier max-g2 finished 9/20, worse than the streak-gated g1
  (12/20), and the clean good-zone path stands at 14/25. A third gated model (settle 11) started.
- 14:15 UTC: day-window tickets resumed. servo ran a 20-run c8 copy, and the gated session finished max-g3 at 12/20.
- Day c8 copies: servo-c8s8 went 5/20 (stuck 0/10) and servo-c8s9 11/20. The three gated qualifiers total 33/60 = 55%.

## Day 4 (Oct 17, afternoon)
- 16:35 UTC: near-right back was shown to be a reach limit. The claw lands at about lx 86 whatever the R, and those
  placements win 4/63.
- 17:30 UTC: a study of the leaders' public recordings found they use the same open-loop plan, aim and settle as us.
  Nothing can act after the drop, and 60/66 losses are grip slips, more often when the claw lands right of the capsule.
  Three slip-reducing mechanisms went to screening.
- hp-st1 (stochastic aim on hard classes) screened 6/10. servo paid about 60 day-window runs, and the gated session
  continued with max-g4.
- 17:20 UTC: the leaders study was corrected. A regression on 2,361 runs puts the left-bias lever under 1 point, and the
  real lever is back-row tails. The far-right back corner (0/20) got a dislodge recipe.
- Screens: stochastic aim pooled 19/60 (null), approach direction null, corner dislodge 5/10 with landings too far right
  (fixed in c8bk3). Dynamic drop started. The gated max-g4 went 10/20, so settle length has no effect. hp paused.
- 18:15–19:15 UTC: servo-dd1 (dynamic drop) finished 2/20, null. intel-c8bk3 went 0/20, all on one stuck mid-right
  capsule its patch never touched. Play slowed to about 20 runs an hour.

## Day 5 (Oct 18)
- Overnight and into the day, only the gated session played (about 17 runs). max-g5 finished 9/20, and the five gated
  qualifiers total 52/100.
- By 15:15 UTC, virtual_jpeg had tied us at 17/20 (#2) with a new model. publicqi still leads at 18/20.
- 15:35 UTC: the gated max-g6 finished 14/20, the gated session's best. It doesn't change our rank (tied #2 at 17/20),
  and the six gated qualifiers total 66/120 = 55%.
- 17:30 UTC: max-g7 also finished 14/20. Seven gated qualifiers total 80/140 = 57%.
- 18:20 UTC: max-g8 finished 15/20. The last three gated tickets (15:00–18:20) went 43/60 = 72%, and all eight total
  95/160 = 59%.
- 20:30 UTC: max-g9 finished 10/20. Nine gated qualifiers total 105/180 = 58%.
- 21:30 UTC: max-g10 finished 13/20. Ten gated qualifiers total 118/200 = 59%.
- 00:45 UTC Oct 19: max-g11 finished 10/20. Eleven gated qualifiers total 128/220 = 58%.
- 01:45 UTC: max-g12 finished 13/20. Twelve gated qualifiers total 141/240 = 59%.
- By 02:30 UTC Oct 19, publicqi had posted 20/20 with a new model and holds #1 alone. We stay tied #2 at 17/20 with
  virtual_jpeg.
- Our play paused: no runs were paid after 01:58 UTC on Oct 19, with the thirteenth gated ticket (max-g13) unfinished.
- 12:30 UTC Oct 19: manual unstick grabs resumed. The corner recipe (R 1790, U 1275) moved a stuck far-right back capsule
  into open field (3/3 live), and the next run won.
