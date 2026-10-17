# 04 – Machine findings

These are the most useful things we learned, mostly from thousands of public job results and purchased
recordings. A key unlock: **a recording's ID is its job ID, and `GET /v1/jobs/{id}` is public,** so recordings
can be joined to outcomes and timing.

## 1. The machine has memory
| State before the run (all players) | Win rate |
|---|---|
| previous run won | **~0.38–0.39** |
| previous run lost | ~0.21 |
| loss streak 1 / 2 / 3 / 4 / 5 | 0.32 / 0.27 / 0.24 / 0.24 / 0.21 |
| loss streak 6+ | **~0.10** |

- **A miss leaves the capsule exactly where it was:** the next start is a median 0.19 px away, and 76% are
  within 1 px (n = 842).
- **A win re-dispenses it at random:** the next start is a median 8.1 px away, spread over the whole pit.
- **Within a 20-run batch, runs are correlated** (lag-1 correlation about 0.19). A back-to-back batch has about
  1.46× the binomial variance, an sd of about 13 points rather than 11.
- **For our own models:** 43% after a win, 36% after a loss streak of 1–2, and **11% after a streak of 3+**.
  45% of our runs were played into stuck capsules.

## 2. Stuck states are mostly one spot
Loss streaks of 3+ (n = 712, 17% win rate) by zone:

| Zone | Share of stuck states |
|---|---|
| Front-green dead zone | 43% |
| Far right | 15% |
| Mid field | 17% |
| Back | 14% |

The **near-right dead zone** is `cx 88–90.5, cy 82–86` with the green half facing the camera. It's nearly
absorbing:
- 209 runs there won 3;
- losing grabs moved the capsule out only 8% of the time.

In frames it looks like an ordinary lying capsule, so the trap is physical, not a perception error.

## 3. Placement zones after a win
mid 38%, back 23%, front 15%, front-green 14%, far right 7% (n = 426).

## 4. Aim bands that win in hard zones (all players)
| Zone | Winning band | Avoid |
|---|---|---|
| Dead zone, front-green | **ey < 2 (shallow)**: 3/29 won | ey ≥ 2: 0/180 |
| Back-right (cx 86–90.5, cy < 86) | **ey 3–4**: 22/85, or ey 1–2: 8/41 | ey ≥ 4: 9/196 |
| Far right (cx ≥ 90.5) | **R 1600–1800**: 9/54 won, 62% escape | R ≥ 2400: 4/50; R < 1600: 0/17 |

## 5. Unstick recipes (for setting up the next run)
Escapes by zone for losing grabs:
- front-green 15%
- far right 45%
- back 40%

After an escape, the next run wins far more often than when the capsule stays put (far right: 30/87 vs 8/99).
- **Far right:** a normal-depth grab at `R 1600–1800`. Landing behind the capsule at the end stop is worse (4/14 escaped).
- **Back-right:** landing behind the capsule (ey < 0) rakes it forward (11/19), but the next run didn't win more often.
- **Sticky front-green back-right spot** (oy ≥ 0.8, cx 87.8–89.6, cy 82.5–84.8; about 8% of all recordings):
  across all players, 82 grabs won once, and standard aims (U 800–1000) move it less than 1 px. **Overshooting
  with U = 1400** (R unchanged) went 3 for 3 live: 1 win and 2 dislodges (7 px and 20 px). The old win rate was
  about 1%, so the rule costs nothing.
- **Widened sticky zone (census of 1,430 decoded recordings, 1.5 px cells).** Every one of the commonest stuck
  cells is a front-green capsule in the near-right area: (88.5, 84) won 2/65, (90, 85.5) 0/56, (90, 84) 2/51,
  (90, 87) 2/28 and (88.5, 82.5) 0/17. 75–90% of losses there leave the capsule unmoved. The original sticky zone
  (cx ≤ 89.6, cy ≤ 84.8) misses the cx ≈ 90 / cy 85–88 cells. In the wider zone (cx 89.2–91.2, cy 83–88.2, n = 152):
  - landing at or behind the capsule (ey < 1) moved it 9/15 and won 2/15
  - ey ≥ 2 moved it 6/130 and won 1/130 (ey ≥ 4: 0/81)

  Proposed rule for front-green, cx 87.8–91.2, cy 82.5–88.2: aim at ey ≈ −3.5, i.e.
  `U = 61.54 · (101.56 + 3.5 − cy)`. Under this census, c8's dead-zone pull (ey 0.75) is borderline, since ey < 0 does better.
- **Front-right corner** (about cx 92, cy 91–94): R 1700 didn't reach it, and a corner recipe of R 2000, U 160
  landed about 8 rows in front of the capsule. The corner recipe was dropped, and far right now uses v2 depth with a
  scaled R.
- **Far-right manual grabs:** R 1700 only reaches about cx 88.9, so the recipe now uses R = max(1700, R_v2 + 80),
  capped at 2600.
- **Deep back-right corner** (cx 88–90.3, cy 80–82): the back-right band data is mostly cy 82–86, so it doesn't cover
  this corner. intel-cb1 went 1/5 there, mostly on one stuck capsule hit repeatedly.
- **Manual grabs** free front-green capsules a bit more often than model grabs (19% vs 14%).

## 6. Aim map (measured landings from video)
- Landing x is measured as the claw column at its lowest point. Two independent readings agree to 0.78 px.
- **Win rate vs measured dx:** < −1: 17/61; −1..0: 18/33; 0..+1: 17/30; +1..+2: 13/17.
- **Mid-field sweet window:** measured dx +0.5..+1.5 won 16/20.
- **v2 already lands in that window on average.** An earlier reading that v2 lands 0.32 px left of target came
  from 13 noisy runs and was wrong. A build that added a +0.75/+1.25 px x shift went 4/20, and its mid-field misses
  all landed right (dx +1.1 to +2.6). The recommended x shift relative to v2 is **0**.
- **Front edge:** cy ≥ 94.5 is 0/6 across all model runs (U 125–254). Wins at cy 93.5–94.5 used U 171–269. That
  row may need less U than any calibration predicts; it's untested.

## 7. Placements don't last
A camera-based gate only helps if the capsule is still where the camera saw it when our run starts. Over 5,835
monitor frames (good-zone rate 6.6%), P(still in the good zone | in it now) was 21% after 15 s, 14% after 30 s and
10% after 60 or 120 s. These are single frames, so the figures include detector noise. With one job queued ahead,
our run starts about 13 s later, and most of the camera's value is gone (about 0.21 · 0.66 + 0.79 · 0.42 ≈ 47%). The gated
session now pays only when no job is unfinished, or when the one running job is at least 9 s old.

## 8. Night
- Sunset to sunrise (about 01:35–14:15 UTC) roughly halves win rates for everyone.
- Absolute colour thresholds misfire at night. Floor-relative features or a narrower ROI fix most of it.
- Field win rate over the logged history: night 701/3,520 = 20%, day 1,293/4,416 = 29%. One leading public model has
  gone 17/27 at night.
- **Near-right, non-front-green capsules** (all players, 124 attempts, 31 wins): ey 3–4 won 14/32, against 5/35 for
  ey < 3 and 12/57 for ey ≥ 4. ex −1.5..0 won 15/38, against 9/44 for ex < −1.5 and 6/42 for ex ≥ 0.5.
