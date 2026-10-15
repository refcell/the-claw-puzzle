# 03 – The policy

Our best policies are hand-built ONNX graphs, not trained networks. With a few hundred recordings and a precise,
repeatable claw, explicit geometry beat learning. Machine learning is used for analysis.

## Pipeline (per run)
1. **Detect** the capsule in the first frame.
2. **Compute** an exact right hold `R` and up hold `U` in ms from its pixel position.
3. **Execute** the holds (in chunks of 1000 ms or less), then **settle**: noop for about 1–1.6 s so the swing dies down.
4. **Drop.**

There's no feedback after frame 0. Exact holds land with an sd of about 1.5–1.7 px. Tracking the claw predicted
the landing worse (sd 3.7 px), and every closed-loop servo we built went 0/4 or worse.

## Detector
- **Red channel is the robust cue:** capsule R is about 40–90, the floor about 130.
- **Region of interest:** x 61–96, rows 80–104. Static false pixels sit at the glass edges (x 58, x 97) and the
  floor edge (y 77–78). Narrowing the ROI from x 60 to **61** fixed our only real night bug.
- **Soft-mask centroid** with a sharp sigmoid (gain about 200). Argmax picks the leftmost column, which biases x
  by about 2 px.
- **Ignore the reflection:** the green blob 8–16 rows above many capsules is the capsule's reflection in the back
  wall. The ROI (y ≥ 80) already excludes it.
- **Face:** the capsule is half opaque green, half clear. If the mean blue of capsule pixels is **B < ~165**, the
  green face is up; otherwise it's teal (clear side up). Absolute colour thresholds drift at night; floor-relative
  thresholds are more robust.
- **Orientation:** whether the green half faces the camera ("front-green", `oy ≥ 0.8`) matters a lot. See below.

## Calibration (fit on model-run wins with exact holds, never manual runs)
```
landing x   = 68.76 + 0.0117 · R
landing row = 101.56 − 0.01625 · U      (sd ≈ 2.3 rows)
```

## v2 rules (`9758e28b`, 12/20 = 60%)
```
R = (cx − 67.73) · (81.9 − 1.1 · (cy − 88))     # perspective cross-term
R = 2600                    if cx ≥ 90.5         # pin to the right end stop
U = 6948 − 71.8 · cy
settle = 10 ticks
```
The cross-term fixed earlier models' biases: back placements landed left and front placements landed right.

## Aim offsets
- `ex` = landing x − capsule x and `ey` = predicted landing row − capsule row.
- Measured from videos, the best `ex` is about **+0.4 to +1.25 px right** overall (+1.5 for teal). Green is
  forgiving from −1.25 to +1.
- **Depth targets:** green `ey ≈ 2.7`, teal `ey ≈ 3.7`.
  - Face-aware rule: `U = 61.54 · (101.56 − ey* − cy)`, with `ey*` ramped on blue.

## Class rules (fg / fl series, hard-placement lab)
- **fg2 (`8fb77333`):** v2 plus a front-green branch, with less U and slightly left when the green half faces the
  camera. It scored 13/21 at first, but about 26% as a long-run control. It's an average policy.
- **fl1 (`f67ecdca`):** fg2 plus three class rules (sketched in code after this list).
  - a shallow dead-zone branch, `ey ≈ 1.2`
  - far right at `R ≈ 1800` instead of the 2600 pin
  - tuned back-right aim

  It's our strongest state-adjusted result: **34/71, +14.7 wins over expectation** (O/E 1.76).
- **Back-row branch (intel bk1):** for cy < 84 and not front-green, aim `ex 0.0 / ey 3.1`:
  `R = (cx − 0.3 − 67.4)/0.0126` and `U = 61.54 · (101.56 − 3.1 − cy)`, ramped by `sigmoid((84.3 − cy)·3)`.

As a rough sketch, the fl1 class branches sit on top of v2's holds like this. The zone boundaries come from
[04 – Machine findings](04-findings.md). The real builder blends branches with soft sigmoid ramps, and its
back-right constants aren't reproduced here.
```
if front_green and 88 ≤ cx < 90.5 and 82 ≤ cy ≤ 86:   # near-right dead zone
    U = 61.54 · (101.56 − 1.2 − cy)                    # shallow grab, ey ≈ 1.2
elif cx ≥ 90.5:                                        # far right
    R = 1800                                           # instead of v2's 2600 end-stop pin
# back-right (cx 86–90.5, cy < 86): tuned aim; the data favours ey 3–4
```

## The best combined candidate on paper
**intel-cb1** is fg5 plus three changes, and is the only policy checked that sits in the winning band for all
three hard zones:
- the back-row branch, `ey 3.1`
- a shallow front-green target, `ey 1.2`
- far right at `R 1780`

It's built but not yet tested live.

## Night
Win rates are about 30% or less at night for everyone, us included. After the ROI fix, our night performance
matched the field's best. Public night runs showed no hidden trick.
