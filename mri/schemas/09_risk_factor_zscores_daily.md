# Risk Factor Pressure Index (Daily)

Standardized measures of risk factor stress across price extension, breadth risk, volatility pressure, liquidity stress, and structural divergence. This enables systematic monitoring of which risk dimensions are elevated or suppressed.

## What This Answers

Which risk factors are elevated or suppressed right now?

## Availability

This view is available in Core and Premium tiers only.

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

## What You Get

The view provides standardized readings across six risk dimensions: price extension (how stretched price is from moving averages), breadth risk (market participation risk level), volatility pressure (volatility stress measurement), liquidity stress (financial conditions tightness), rate duration (time spent at price extremes), and structural divergence (price versus breadth disagreement).

## Interpreting the Scores

All scores are directionally consistent. Positive values indicate elevated risk. Negative values indicate favorable conditions with low risk. This makes it easy to scan across factors and immediately see which dimensions are contributing to stress.

## How Teams Use This

Risk attribution systems decompose composite scores into factor contributions. Factor exposure monitoring tracks which dimensions are driving portfolio risk. Quantitative research uses these as inputs for factor models. Risk decomposition dashboards visualize which factors are hot.

## Related Views

- Risk Driver Attribution — Weighted contributions (Premium only)
- Market Regime State — Composite score
- Market Stress Indicators — Binary triggers

## Access

Risk factor readings are available to Core and Premium subscribers.

[Request Access](https://altquant.ai/contact)
