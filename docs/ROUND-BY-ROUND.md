# Round-by-round analysis

This document explains the retained code and result evidence at a level suitable for review without publishing the complete team submissions.

## Round 1 — Position targets and forensic replay

**Retained official submission:** `270996.py` (533 lines)

**Products:** `INTARIAN`, `ASH`

**Official simulated result:** 101,448.75 Xirec

The main architecture loaded prior state, converted timestamp and price observations into a target position, and generated order-book ladders to move toward that target. The strongest retained behaviour was rapid accumulation of `INTARIAN` toward a +80 position, with `ASH` providing a secondary strategy.

The later forensic replay did more than report aggregate PnL. It checked when the position target was reached, where gains flattened, and whether the behaviour came from a repeatable rule or a narrow path in the data. The replay showed the +80 target was reached by timestamp 1,000 and exposed a structural plateau afterwards.

**Decision:** retain the explainable target-position mechanism, but stop treating the headline replay result as sufficient evidence of robustness.

## Round 2 — Trend, fair value, and backtester disagreement

**Retained official submission:** `280624.py` (532 lines)

**Explainable candidate:** 183 lines

**Official simulated result:** 9,436.31 Xirec

The explainable candidate combined:

- fast and slow exponential moving averages;
- rolling volatility;
- microprice and order-book imbalance;
- trend logic for `INTARIAN`;
- fair-value / market-making logic for `ASH`.

A separate `MAF` idea was investigated but was not active in the candidate's final return path. This matters because comments, unused branches, and executed behaviour are not interchangeable evidence.

The decisive finding was methodological: a public local backtester could produce a different answer depending on horizon and simulator assumptions. Replaying the retained official file on the audited community backtester returned 0, while the official bundle recorded 9,436.31. That makes Round 2 a compatibility warning, not a clean local reproduction.

**Decision:** compare candidates under identical local conditions, but never replace retained official output with a local number.

## Round 3 — Options, continuation, and a misleading preview

**Retained official submission:** `487619.py` (268 lines; large because route data was embedded)

**Products:** `VELVETFRUIT_EXTRACT`, VEV option strikes, `HYDROGEL`

**Official simulated result:** 67,135.87 Xirec

The implementation included path detection, an oracle/continuation path, fallback orders, and precomputed route information. A visible 100k prefix initially looked like strong evidence, but the final one-million-timestamp run continued beyond that prefix.

The retained postmortem attributes the failure to stale fair-value anchors and long option exposure during repricing and time decay. The path guard fell back rather than preserving the expected continuation logic. In other words, the failure was not just “a bad parameter”; it was a mismatch between the model's assumptions and the later market path.

The audited community replay returned 169,490 versus 67,135.87 officially, reinforcing that local replay could materially overstate the official outcome.

**Decision:** investigate PnL by product and time segment, and treat a partial preview as a test artifact rather than a final result.

## Round 4 — Volatility, vertical spreads, and counterparty flow

**Retained official submission:** `521226.py` (349 lines)

**Official simulated result:** 8,818.17 Xirec

The retained implementation contains:

- a normal cumulative distribution approximation;
- Black–Scholes call valuation;
- vega and implied-volatility calculations;
- option vertical-spread construction;
- product-specific trading functions;
- analysis of trades involving the counterparty identity `Mark`.

Research compared following and fading counterparty flow and examined whether option relationships remained coherent. The local full-day replay returned 50,918 under the community backtester's default matching mode. Alternative matching changed the total only modestly, while the gap to the official 8,818.17 remained large.

The uploaded performance graph represented only the first tenth of a day, so it could not be used as proof of full-round performance.

**Decision:** retain product-level and volatility diagnostics; reject conclusions based on a partial graph or full-day local result alone.

## Round 5 — Ten categories, four retained

**Retained candidates:** `Reflection-Aligned Candidate.py` (100 lines) and `Defensible Final Candidate.py` (163 lines)

**Official result bundle:** not retained

The evaluation covered ten product categories using mean-reversion, momentum, order-book-imbalance, and breakout variants. The analysis compared gross and friction-adjusted results, stability across days, and whether each category's logic could be defended.

Four categories survived the final screen:

- `OXYGEN_SHAKE`
- `ROBOT`
- `SNACKPACK`
- `UV_VISOR`

Six were excluded after costs or robustness checks. Both retained candidate files produced the same 57,522 total on the audited community replay, but there is no retained official upload bundle with which to validate that number.

**Decision:** publish the selection logic, not an unsupported Round 5 performance claim.

## Overall lesson

The most useful artifact was the evaluation process: hypothesis, controlled replay, sensitivity check, official comparison, failure analysis, and a documented decision. Complexity mattered only when it improved that chain of evidence.
