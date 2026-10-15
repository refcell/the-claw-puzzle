# 07 – Strategy and ethics

## Ungated vs gated play
- **Most of our models run ungated:** every paid run happens whenever the session is ready, so the score
  reflects typical play.
- **One session ("max") may time its runs,** with explicit sign-off from the human owner. It uses public signals:
  - **Streak gate:** pay only when the last finished run on the machine (any player) won, or the loss streak is
    at most 1, and at most one job is ahead. On historical data, our models win about 43% after a win against
    11% after a streak of 3+.
  - **Camera veto:** skip known-bad placements (front-green dead zone, far right, against the back wall, narrow
    or upright) when a clean frame is available. On its own data, camera-pass runs won about 66%.
  - Every gated model is tagged `max-` so its scores aren't mistaken for typical play.
- **Lesson:** gating on the camera alone failed. Jobs queued ahead kept moving the capsule before ours ran. Stuck
  states persist through other players' runs, which makes the streak signal sturdier.

## Manual unsticking
- Manual games don't count for the leaderboard, but they're allowed and pay prizes.
- When the machine is stuck, the max session may buy a scripted manual turn designed to knock the capsule out
  of a dead zone (recipes in [04](04-findings.md)). A win also re-dispenses the capsule. Then the gated qualifier
  plays the fresh placement.
- We measure whether it actually frees placements, and drop it if not.

## Portfolio
- Since ties share rank and only the best model counts, sessions run every solid candidate to a full 20 runs.
- **Target 14/20** to tie the current leaders.
- Batches are never abandoned early.

## Run timing and cost
- Prices rise 25% per pending job, and the starting price falls 5% every 30 s while the queue is empty, so a busy queue is
  expensive.
- More of our own sessions don't add throughput. There's one machine.
- More wallets don't help either. The board is keyed to the player account, and the queue is FIFO on one machine.

## What we won't do
- **No extra accounts or identities** to add leaderboard entries.
- **No forcing refunds or voids** to drop losses.
- **No interfering with other players' runs** or the public queue beyond our own paid turns.

Anything odd we notice during normal play, such as scoring, pricing or timing quirks, gets written down as
feedback for the puzzle's makers.
