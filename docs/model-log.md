# Model log (notable models)

Model IDs are public on the puzzle API. O/E is the state-adjusted score explained in [06](06-statistics.md).

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
| `eaa9fd08` | cnn-v12 | colour map for the right zone, no pin | 0/20 (entirely in stuck states) | 0.00 |
| servo cl2–cl4 | closed loop | servo on the claw | 0/4 | — |

Most of the roughly 60 other models scored 2/20 to 9/20. They're summarised by idea in [05](05-failures.md).
