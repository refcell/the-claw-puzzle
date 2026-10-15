# The Claw Puzzle: a field notebook

This is a running write-up of how we're attacking Paradigm's
[Claw puzzle](https://www.paradigm.xyz/puzzles/claw/). Players upload a small ONNX policy, and it drives a
**real, physical claw machine** through a camera for 15 seconds with a single drop. The leaderboard ranks each
player by their best model's success rate over at least 20 paid runs.

We play as `andreaslbigger`. The work is done by a team of autonomous AI coding-agent sessions running in
parallel, steered by a coordinator agent and a human. This repo records what we tried, what helped, what
failed, and what we learned about the machine. It's updated as the work goes on.


## Where things stand

_As of Oct 15, 21:30 UTC. Public board: publicqi and virtual_jpeg tied #1 at 14/20, us #3 at 12/20, then brockjelmore 9/22 and ethanmlam 13/38._

| | |
|---|---|
| Best qualifying model | `9758e28b` ("cnn-v2"), 12/20 = 60%, #3 behind two models tied at 14/20 = 70% |
| Earlier peak | #1 at 60% on day one; `8fb77333` (hp-fg2) briefly held #3 at 13/21 |
| Paid runs so far | well over 2,000 across about 60 models |
| Net money | roughly break-even: prizes and training-data sales offset entry fees |

## The short version

**What helped**
1. **Open-loop dead reckoning.** Find the capsule once in the first frame, convert its pixel position into
   exact right/up hold times, wait for the swing to settle, then drop. Exact holds land within about 1.7 px.
   Every closed-loop attempt did worse. ([policy](docs/03-policy.md))
2. **Calibrating on our own model runs, not manual play,** and a perspective cross-term in the right-hold formula.
3. **Placement-class rules** for the capsule's face (green vs clear side), its orientation, and three hard
   zones (a near-right dead zone, back-right, far-right).
4. **Discovering the machine has memory.** A miss leaves the capsule where it was, and a win re-dispenses it at
   random. P(win | previous run won) is about 0.39, against 0.21 after a loss and about 0.10 after six or more
   losses in a row. This reframed everything. ([findings](docs/04-findings.md))
5. **State-adjusted scoring.** Each run is compared with what an average policy would score in the same machine
   state, instead of using raw 20-run rates. ([statistics](docs/06-statistics.md))

**Where we failed**
- Closed-loop servoing, a CNN localizer in production, and a learned offline evaluator (no predictive validity).
- Grip-quality ideas: sway timing, settle length, approach direction, a reverse tap, longer settle. All null.
- Paired A/B testing with a fixed control. It had an ordering bias and 80% concordant pairs.
- Treating 20-run leaderboard rates as skill. Most of the spread is placement luck.
- Operations: crowding the single machine with our own runs, and agent sessions crashing on oversized logs.

See [failures](docs/05-failures.md) for the details.

## Contents

| Doc | What's in it |
|---|---|
| [01 – The puzzle](docs/01-the-puzzle.md) | Rules, model contract and pricing, from the public docs |
| [02 – How we work](docs/02-how-we-work.md) | Multi-agent setup, budgets, payment safety, coordination |
| [03 – The policy](docs/03-policy.md) | Detector, formulas, class rules and the night fix, with exact constants |
| [04 – Machine findings](docs/04-findings.md) | Machine memory, placement mechanics, stuck zones, aim maps, unstick recipes |
| [05 – Failures](docs/05-failures.md) | Everything that didn't work, and why |
| [06 – Statistics](docs/06-statistics.md) | Noise, the leaderboard as a lottery, state adjustment, pairing pitfalls |
| [07 – Strategy and ethics](docs/07-strategy.md) | Gating, the portfolio approach, manual unsticking, and what we won't do |
| [08 – Economics](docs/08-economics.md) | Spend, prizes, the training-data market |
| [Model log](docs/model-log.md) | Notable models and results |
| [Timeline](docs/timeline.md) | What happened, when |
