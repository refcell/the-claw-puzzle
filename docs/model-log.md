# Model log (notable models)

O/E is the state-adjusted score explained in [06](06-statistics.md).

| Model | Name | Idea | Result | O/E |
|---|---|---|---|---|
| `e3751a87` | cnn-v0 | colour centroid → open-loop R/U, settle 8 | 10/20 | 1.97 |
| `9758e28b` | cnn-v2 | v0 + perspective cross-term + far-right end-stop pin | **12/20 (our board entry)** | 1.91 |
| `8fb77333` | hp-fg2 | v2 + front-green branch; later used as the control | 13/21 at first, 55/208 overall | 1.03 |
| `f67ecdca` | hp-fl1 | fg2 + dead-zone, far-right and back-right class rules | 34/71 | **1.76, +14.7** |
| `ccb075b1` | lq-b3 | learned P(win) model, argmax holds | 12/20 | 1.87 |
| `e34f3570` | lq-b2 | learned P(win) model | 12/20 | 1.84 |
| `d39ce963` | lq-c5 | c4 + deeper aim in the dead zone | 12/30 | 1.81 |
| `19bfd79e` | v5 | red-channel detector, fit on model-run wins | 11/20 | 1.67 |
| `bba518a8` | intel-bk1 | back-row aim ex 0 / ey 3.1 | 11/30 | 1.36 |
| `cc16ca7b` | v3c-p4 | standing capsules aimed left | 9/20 | 1.56 |
| `29b7a180` | servo-ol1 | open loop, U targets ey ≈ 2.65 | 9/20 | 1.40 |
| `506aad7f` | lq-c8 | c5 + far right at R 1780 instead of the 2600 pin | 9/20 (stuck states 4/8, our best stuck split) | — |
| `ab0bef8c` | v3c-c5 | combined-v3 line | 8/20 | 1.42 |
| `1f8c4a07` | intel-cb1 | every hard zone in its winning band | 8/20 (15 runs in stuck states) | lift +0.089, z 0.87 |
| servo-cb1 | servo-cb1 | servo-line build of the same combo | 6/20 | — |
| `b52eefac` | lq-c7 | c5 without the deeper mid-field aim | 6/20 | — |
| `cf5bdf31` | cnn-v24 | re-roll of the v2 rule | 6/20 | — |
| `c9d347d7` | hp-fl7 | fl1 + sticky-spot dislodge (U 1400) | 5/20; sticky rule 3/3 | — |
| `6a432ecb` | cnn-v25 | v2 x/pin + face-aware U (green 2.7 / teal 3.7) | 3/20 (fresh 3/14); didn't repeat day one's 8/16 | — |
| hp-fl4 | hp-fl4 | fl1 + back-right non-front-green at ey 3.25 | 5/20 | — |
| `6643d0e3` | servo-combo2 | v2 + x shift + face-aware U | 4/20 (x shift retracted) | — |
| `eaa9fd08` | cnn-v12 | colour map for the right zone, no pin | 0/20 (entirely in stuck states) | 0.00 |
| servo cl2–cl4 | closed loop | servo on the claw | 0/4 | — |

**Running now:** hp-fl8 (`eab59850`, fl1 + sticky dislodge + back-row rule), intel-flbk (`25c6db79`, fl1 +
back-row rule), v3c-fl1r (`220e2f0f`, fl1 re-roll), lq-c8r / c8r2 (`28823771`, `8ad9b56a`), lq-c5r2 (`6d995201`),
cnn-v25 (`6a432ecb`, face-aware U green 2.7 / teal 3.7) and servo-fg2n (`e7e482d3`).

Most of the roughly 60 other models scored 2/20 to 9/20. They're summarised by idea in [05](05-failures.md).
