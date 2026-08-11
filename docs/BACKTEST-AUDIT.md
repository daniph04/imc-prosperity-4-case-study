# Backtest audit

## Purpose

The audit asked a narrow question: **Can the retained submissions be reproduced on a public backtester using the same market-data files?** The answer was no. The exercise is still useful because it makes the limits of local testing measurable.

## Environment

- Audit date: 11 August 2026
- Community project: [`nabayansaha/imc-prosperity-4-backtester`](https://github.com/nabayansaha/imc-prosperity-4-backtester)
- Package version: `1.0.1`
- Audited commit: `0094c681f8cd019889761e6431a1a47ea151aaa8`
- Data: 30 price/trade CSV files covering Rounds 1–5
- Data verification: every retained official CSV was SHA-256 identical to the corresponding file bundled by the community project

This project is not maintained or endorsed by IMC. Its own README describes the matching engine as appearing consistent with the official one; that is not a guarantee of identical execution.

## Results by day

| Round / candidate | Day 1 | Day 2 | Day 3 | Local total | Retained official |
|---|---:|---:|---:|---:|---:|
| R1 official submission | 0 (day -2) | 3,066 (day -1) | 6,407 (day 0) | 9,473 | 101,448.75 |
| R2 official submission | 0 (day -1) | 0 (day 0) | 0 (day 1) | 0 | 9,436.31 |
| R3 official submission | 51,114 (day 0) | 66,383 (day 1) | 51,994 (day 2) | 169,490 | 67,135.87 |
| R4 official submission | 34,444 (day 1) | 5,700 (day 2) | 10,773 (day 3) | 50,918 | 8,818.17 |
| R5 reflection-aligned candidate | 20,837 (day 2) | 13,815 (day 3) | 22,870 (day 4) | 57,522 | Not retained |
| R5 defensible candidate | 20,837 (day 2) | 13,815 (day 3) | 22,870 (day 4) | 57,522 | Not retained |

Rounded local values are shown because the audit is about scale and reproducibility, not false decimal precision.

## Command pattern

The retained files were executed with the community project's installed CLI, pinned source checkout, and extracted official data. The general command was:

```text
prosperity4btest <retained-submission.py> <round> \
  --data <audited-official-data-root> \
  --merge-pnl \
  --match-trades all \
  --no-out
```

Matching sensitivity was checked by repeating the run with `--match-trades worse` and `--match-trades none`. Round 5 was run once for each retained candidate. The audit used isolated copies of the submissions and data; it did not modify the private evidence archive.

Input equivalence was checked by calculating SHA-256 for each extracted CSV and comparing the round/day/type pairs before execution. A matching filename alone was not treated as sufficient.

## Matching-mode sensitivity

The community engine was run with `all`, `worse`, and `none` matching modes.

- Rounds 1 and 3 were unchanged across the tested modes.
- Round 2 remained at zero.
- Round 4 returned 50,918 under `all` and `worse`, and 49,233 under `none`.

The matching mode altered Round 4 modestly, but it did not explain the much larger gap from the official result. Horizon, state, product behaviour, or platform-specific execution assumptions were more consequential.

## Drawdown observations

The audit also calculated peak-to-trough changes in the local replay:

- R1: approximately 394,086
- R3: approximately 12,909
- R4: approximately 16,840 under default matching
- R5: approximately 30,701

The very large Round 1 drawdown is another reason not to present its local aggregate as a robust performance result.

## What the local backtester can and cannot establish

It can help with:

- comparing controlled variants on identical files;
- checking whether code runs across all supplied days;
- detecting position, drawdown, and product-concentration problems;
- rejecting signals that fail after costs;
- producing reproducible debugging evidence.

It cannot establish:

- the official score;
- expected live-market performance;
- that matching, state, or horizon exactly mirrors IMC's environment;
- that a retained candidate was the file actually uploaded when no official bundle exists.

## Reproducibility conclusion

The official result bundles are the source of truth for Rounds 1–4. The audited community replay is a diagnostic layer. Its disagreement is documented because hiding it would make the case study weaker, not stronger.
