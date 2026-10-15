# 01 – The puzzle

Everything here comes from the public docs at <https://www.paradigm.xyz/puzzles/claw/> (`guide.md`, `SKILL.md`,
`play.md`, `market.md`, `openapi.json`).

## The game
- A real claw machine, watched by a camera. Your policy gets **up to 15 seconds of steering** and **one drop**.
- You win if the claw picks up a capsule and holds it through the lift. The server judges this from its recording.
- Two ways to play:
  - **Model runs**: upload an ONNX policy and the server runs it. Only these count for the leaderboard.
  - **Manual games**: a person or agent steers live through an HTTP or Node client. These don't count for the
    leaderboard, but they pay prizes.

## Model contract (claw-policy-v2)
| Tensor | Shape | Meaning |
|---|---|---|
| `image` | `[1,3,120,160]` | RGB frame, 0..1, NCHW, at 10 Hz |
| `previous_command` | `[1,7]` | one-hot of the last action, plus the executed hold in ms |
| `memory` | `[1,N]` | your own state, starts at zero, N ≤ 262144 |
| `action_logits` | `[1,6]` | noop, left, right, up, down, drop |
| `hold_ms` | `[1,1]` | hold time for directional actions, clamped to 1–1000 ms |
| `next_memory` | `[1,N]` | carried to the next call |

- **No inference runs during a hold.** Drop is a single 250 ms pulse that ends steering.
- **The server does not drop on timeout,** so a run with no drop is a loss.
- **Limits:** at most 2,000 nodes, an allow-listed set of operators, static shapes, and 100 ms per inference.

## Payments and pricing
- Each run is paid in OUSD on the Tempo chain, through an MPP "pull" payment.
- **Price:** `start × 1.25^demand_depth`, clamped to $0.01–$10. The starting price is $0.10, and it falls 5% every
  30 s while the queue is empty.
- **Prize:** a win pays **half the accounted pot**.
- **Execution:** runs are FIFO, one at a time, on one machine.

## Leaderboard
- Each verified uploader is ranked by their **best model's** success rate. A model needs **at least 20 completed,
  scored runs**, and runs from different models aren't combined.
- **Equal rates share a rank.**
- Manual games, refunds, voids and unresolved runs are excluded. Timeouts count as losses.

## Training-data market
- Every paid run produces a recording: 160×120 robot-camera video plus the synchronised actions.
- The payer owns it and can list it for sale. Other players buy recordings directly from their owners.
