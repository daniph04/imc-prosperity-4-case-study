# IMC Prosperity 4 — Strategy Evaluation & Backtesting

An evidence-backed case study from IMC's five-round international trading challenge. The central problem was not finding the most complicated signal; it was deciding which results were trustworthy after spreads, transaction costs, and differences between local backtests and official runs.

> **Verified team result:** qualified for the finals and finished **750th of 18,803 teams worldwide (top 4%)**.

![IMC Prosperity 4 official overview](imc-prosperity-4-official-overview.png)

## The challenge

Each round combined an algorithmic submission with a manual challenge in a simulated market. Our team needed a repeatable way to compare ideas, tune parameters, identify misleading results, and make a final trading decision under a deadline.

![Prosperity 4 competition scale](competition-scale-clean.jpg)

The scale image is a privacy-safe crop of the retained official IMC results page. Only browser chrome was removed; the published competition figures were not altered. IMC's [current Prosperity 4 page](https://prosperity.imc.com/) independently confirms 18,803 participating teams and the five-round format.

## My contribution

- Worked across strategy research, Python implementation, and backtesting during all five rounds.
- Compared candidate strategies against historical CSV data and official run logs instead of treating local backtests as ground truth.
- Used parameter-search and risk-audit workflows to test sensitivity to lookback windows, thresholds, bid-ask spreads, and transaction costs.
- Summarized uncertainty and helped the team decide which products not to trade.

Implementation and experimentation were substantially assisted by AI coding tools. The value of the work was in framing the tests, checking the evidence, interpreting failures, and turning the results into a team decision.

## The decision that mattered

In the final round, the team evaluated signals across ten product categories. Six did not remain viable once market frictions were included, so I supported narrowing the final scope to four rather than forcing the strategy to trade every category.

The lesson was simple: a promising signal is not enough. Execution costs, robustness across days, and the decision not to trade can matter more than model complexity.

## Verified context and result

- IMC describes Prosperity 4 as a five-round challenge with algorithmic and manual components.
- IMC's official site records **18,803 participating teams**.
- The retained final leaderboard records the team at **#750 overall**, **#951 algorithmic**, **#749 manual**, and **#9 in country**.

![Retained Prosperity 4 final leaderboard](final-leaderboard-clean.jpg)

The leaderboard image is a privacy-safe crop of the original official results screen. Only browser chrome was removed; the team name, rankings, and XIREC value were not altered.

![IMC Prosperity 4 finals qualification](qualified-for-finals.png)

## What this case study demonstrates

- Python-based analysis and backtesting
- Experimental design and parameter testing
- Evidence-based decision-making under uncertainty
- Transaction-cost and spread sensitivity
- Clear separation of individual contribution from team results

## Boundaries

- Rankings and competition PnL are team results from a simulated market, not individual or real-money performance.
- This repository is a concise case study, not the team's trading code or private working materials.
- The exact Round 5 uploaded file was not retained in an official result bundle, so no claim depends on reconstructing it.
