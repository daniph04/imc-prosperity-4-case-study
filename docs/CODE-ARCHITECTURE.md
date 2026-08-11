# Code architecture

This is a structural explanation of the retained code. Complete team submissions remain private.

## Round 1 and Round 2 architecture

The final files used a stateful target-position architecture:

```text
load prior state
    ↓
read timestamp, positions, and order books
    ↓
derive a target position from the observed path
    ↓
interpolate the target through time
    ↓
build buy/sell ladders subject to position limits
    ↓
persist state for the next invocation
```

The explainable Round 2 candidate separated signal components more explicitly:

```python
trend = fast_ema - slow_ema
risk_scale = f(rolling_volatility)
book_signal = f(microprice, imbalance)
target = combine(trend, book_signal, risk_scale)
orders = move_toward(target, current_position, position_limit)
```

This pseudocode describes the executed structure without reproducing team code.

## Round 3 architecture

Round 3 used options and underlying relationships plus precomputed route information:

```text
detect whether the observed path matches a retained route
    ├─ match: continue the planned sequence
    └─ no match: fall back to general pricing and risk logic

price underlying and option relationships
    ↓
construct orders within product limits
    ↓
attribute PnL and diagnose continuation/fallback behaviour
```

The postmortem showed why architecture matters: a guard that falls back safely can still produce poor economics if its fair-value anchors are stale.

## Round 4 architecture

The Round 4 file contained explicit quantitative components:

```text
normal CDF
Black–Scholes call value
vega
implied-volatility solver
vertical-spread construction
counterparty-flow feature (`Mark`)
product-specific order generation
```

The evaluation separated three questions:

1. Is the theoretical option relationship coherent?
2. Does observed flow improve the decision to follow or fade?
3. Does the result survive the official environment and full horizon?

## Round 5 architecture

The final selection workflow treated each category as an independent decision:

```text
for each product category:
    test mean reversion
    test momentum
    test order-book imbalance
    test breakout behaviour
    apply spread / transaction-cost assumptions
    compare across supplied days
    retain only if the result is stable and explainable
```

The shorter reflection-aligned candidate and the longer defensible candidate produced the same audited local total. That agreement supports implementation consistency; it does not prove official performance.

## Engineering practices demonstrated

- separating signal generation from risk and order construction;
- persisting and validating state;
- testing multiple days rather than one favourable slice;
- comparing product-level PnL and position paths;
- using hashes to verify test inputs;
- documenting unused or unexecuted logic separately from active behaviour;
- treating simulator disagreement as a defect to investigate.
