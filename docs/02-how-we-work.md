# 02 – How we work

## The team
We run several **autonomous AI coding-agent sessions** in parallel. Each has its own git worktree and a research
focus, coordinated by one agent and a human:

| Session | Focus |
|---|---|
| cnn | Vision-first policies (colour detector plus a CNN localizer), the v2 family |
| servo | Closed-loop servoing, then aim maps, night perception, grip experiments |
| v3c | Combining the best rules, an offline evaluator, the machine-state tool |
| hp (hard-placement lab) | Hard placement classes: front-green orientation, back-right, far-right (fg/fl series) |
| intel | Competitor and market-data analysis, placement mechanics, unstick recipes |
| lq | A learned P(win \| first frame, R, U) model with argmax holds |
| max | "Max reliability": anything legitimate toward 100%, including run timing and manual unsticking |

A **watchdog** job runs every 15 minutes. It nudges idle sessions and stands down when the wallet runs low.
Sessions share findings through a common `SYNTHESIS.md` and their own `NOTES.md` files.

## Standing rules every session follows
- **Payments are serialized** with a filesystem lock. Every spend is logged with a session tag, and each session
  has a budget cap.
- **Hard limits per run:** price ≤ $0.10, at most $1 per run, and no automatic payment retries.
- **Never pay twice on an unresolved quote.** Reconcile it through `GET /v1/quotes/{id}` first, as the docs
  require. Several sessions hit stale or empty challenges and handled them without double-paying.
- **Run queue:** at most 1–2 queued runs per session. The machine is shared with everyone.
- **Secrets:** the API key never appears in logs, commits or command traces. One session caught a `set -x` that
  would have printed it, and removed it before it ran.
- **Hypothesis first:** each new model ID records the change and its hypothesis.
- **Finish qualifiers:** a 20-run qualifying batch, once started, is always finished.

## Coordination lessons
- **Shared machine, shared state.** At one point our own sessions made up about 90% of all runs on the machine.
  That crowded out our timing experiments and raised the price. We later cut to four paying sessions, then
  scaled back up.
- **Quiet windows.** When the timing session wanted an idle machine, it touched a `.maxwindow` file and the other
  sessions paused new payments for up to 180 s. Other players' runs still slipped in, as they should.
- **Session crashes.** Agent sessions died when a single log line grew too large, from huge tool outputs. Keeping
  chat and tool output small became a rule, and crashed sessions were replaced by fresh ones that read the old
  notes.
- **Automation boundaries.** A watchdog that started answering strategy questions "as coordinator" had to be
  limited to nudging.
