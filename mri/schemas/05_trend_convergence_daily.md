# Trend Alignment Index (Daily)

Measures agreement between price, breadth, and volatility trends to detect divergences early. This identifies whether rallies are healthy and convergent or showing warning signs through divergence before breakdowns occur.

## What This Answers

Is the trend healthy or showing warning signs?

## Availability

This view is available in Core and Premium tiers only.

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

## What You Get

The view provides price trend analysis (direction and strength), breadth trend analysis (direction of market participation), volatility trend analysis (direction of risk conditions), divergence detection for price/breadth disagreements, and trend character classification summarizing overall trend quality.

## Trend Character Values

**BEARISH DIVERGENCE** — Price rising while breadth falling. This is a warning sign that the rally lacks broad support.

**BULLISH DIVERGENCE** — Price falling while breadth rising. Often seen near potential bottoms.

**WARNING** — Price rising alongside rising volatility. Indicates an unstable rally.

**CONVERGENT UP** — Both price and breadth rising together. This is a healthy uptrend with broad participation.

**CONVERGENT DOWN** — Both price and breadth falling together. Confirmed downtrend.

**MIXED** — No clear pattern. Conditions are indeterminate.

## Trend Confirmation

**CONFIRMING** — Price, breadth, and volatility are all aligned. Strong signal.

**DIVERGING** — Price and breadth moving in opposite directions. Warning signal.

**MIXED** — Partial alignment across indicators.

## How Teams Use This

Trend-following strategies use this to confirm trend quality before adding exposure. Mean-reversion timing strategies watch for divergences as setup conditions. Rally and correction quality assessment relies on convergence readings. Risk management overlays use divergence detection as an early warning layer.

## Related Views

- Market Participation Metrics — Breadth details
- Market Breadth Structure — Phase analysis
- Market Regime State — Regime context

## Access

Trend convergence analysis is available to Core and Premium subscribers.

[Request Access](https://altquant.ai/contact)
