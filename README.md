# IMC Prosperity 4 — Strategy Evaluation, Backtesting & Failure Analysis

An evidence-backed case study of five rounds of algorithmic trading in IMC Prosperity 4. The work was not simply to produce a trading bot. It was to decide which signals, parameters, and local results were reliable enough to use when spreads, position limits, time horizons, and the official simulator could materially change the outcome.

> **Verified team result:** finalist · **#750 of 18,803 teams worldwide (top 4%)** · **#9 in Spain** · **#951 algorithmic** · **#749 manual**.

![Official final leaderboard showing the team result](final-leaderboard-clean.jpg)

## What I did

I worked across strategy research, Python implementation, backtesting, parameter testing, and post-run analysis. My most defensible contribution was building and using the evaluation workflow: replaying candidates on the supplied CSV data, comparing ideas across days, testing whether apparent gains survived market frictions, and reconciling local output with retained official logs.

The code was developed and iterated with substantial assistance from AI coding tools. I set the hypotheses and acceptance criteria, inspected the implementations, ran the tests, investigated failures, and used the evidence to make strategy decisions. Rankings and competition profit figures are team results in a simulated market, not individual or real-money performance.

## The five-round progression

| Round | Market problem | Main implementation | What the evidence changed |
|---|---|---|---|
| 1 | Position accumulation in `INTARIAN` and secondary trading in `ASH` | Target-position interpolation, order-book ladders, and persisted state | Forensic replay showed how quickly the bot reached its +80 target and where performance plateaued. |
| 2 | Trend and fair-value signals under a different product regime | EMA, volatility, microprice, imbalance, and market-making experiments | The local backtester could show attractive full-horizon results that did not reproduce in the official environment. |
| 3 | Options and underlying relationships | Path detection, continuation logic, oracle/fallback orders, and embedded route data | A strong preview was not the final result. Stale anchors and time decay exposed a failure in assumptions, not merely parameters. |
| 4 | Volatility, vertical spreads, and counterparty behaviour | Black–Scholes calls, vega, implied volatility, spread construction, and `Mark` flow analysis | Full-day local replays materially overstated the retained official result; horizon and simulator behaviour dominated small tuning gains. |
| 5 | Selecting robust strategies across ten categories | Mean reversion, momentum, order-book imbalance, breakout tests, and cost-aware filtering | Six categories were excluded after friction and robustness checks; four remained defensible: `OXYGEN_SHAKE`, `ROBOT`, `SNACKPACK`, and `UV_VISOR`. |

The complete technical explanation is in [Round-by-round analysis](docs/ROUND-BY-ROUND.md) and [Code architecture](docs/CODE-ARCHITECTURE.md).

## Reproducing the backtest audit

I re-ran the retained submissions against all 30 official market-data CSV files using the open-source [`imc-prosperity-4-backtester`](https://github.com/nabayansaha/imc-prosperity-4-backtester), version `1.0.1`, commit `0094c681f8cd019889761e6431a1a47ea151aaa8`. SHA-256 comparison confirmed that its bundled price/trade files were byte-identical to the official data retained for this project.

That backtester is **community-built, not an official IMC tool**. The official competition platform and retained result bundles remain authoritative.

| Round | Community replay total | Retained official result | Interpretation |
|---|---:|---:|---|
| 1 | 9,473 | 101,448.75 | Large environment or horizon mismatch. |
| 2 | 0 | 9,436.31 | Submission behaviour was not reproduced locally. |
| 3 | 169,490 | 67,135.87 | Local replay substantially overstated the official outcome. |
| 4 | 50,918 | 8,818.17 | Matching-mode changes were small relative to the official gap. |
| 5 | 57,522 | Not retained | Useful for candidate comparison only; no official comparator exists. |

This discrepancy is one of the most important findings in the project. A local backtest was useful for rejecting weak ideas and comparing controlled variants, but it was not proof of expected official performance. See [Backtest audit](docs/BACKTEST-AUDIT.md) for per-day results, matching modes, limits, and the exact verification method.

## The decision that mattered

Round 5 tested ten product categories across multiple signal families. Six did not remain convincing after spreads, transaction costs, cross-day stability, and implementation risk were considered. The defensible configuration retained four instead of forcing every category to trade.

The practical lesson was not “find the most complex signal.” It was:

1. make the hypothesis testable;
2. compare it across days and regimes;
3. include execution frictions;
4. investigate disagreement with official output;
5. allow **do not trade** to be a valid result.

## Evidence

![Official finals qualification screen](qualified-for-finals.png)

- [Full evidence brief (PDF)](Daniel-Pena-IMC-Prosperity-4-Evidence-Brief.pdf)
- [Evidence, provenance, and claim limits](docs/EVIDENCE-AND-LIMITS.md)
- [Round-by-round analysis](docs/ROUND-BY-ROUND.md)
- [Backtest audit](docs/BACKTEST-AUDIT.md)
- [Code architecture](docs/CODE-ARCHITECTURE.md)

IMC's [Prosperity 4 page](https://prosperity.imc.com/) independently confirms the five-round format and 18,803 participating teams.

## What this demonstrates

- Python-based strategy evaluation and debugging
- Backtesting with explicit limits and reproducibility checks
- Parameter search, sensitivity testing, and transaction-cost analysis
- Options, volatility, order-book, and market-microstructure reasoning
- Failure analysis and evidence-based scope reduction
- Clear separation of individual contribution, team result, and AI assistance

## Public boundaries

This repository explains the work without publishing the team's complete submissions, private working archive, or any secret. Round 5 has no retained official result bundle, so no claim depends on reconstructing an upload. Competition profit values are simulated Xirec, not dollars or investment returns.
