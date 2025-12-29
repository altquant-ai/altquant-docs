# Risk Driver Attribution (Daily)

Factor-level attribution showing which risk components are driving the current composite risk level. This provides the explainability layer for investment committee reporting and audit-grade documentation.

## What This Answers

Why is the risk level at its current state?

## Availability

This view is available in Premium tier only.

## Data Structure

One row per trading day per index per factor. Primary key is market_date + index_name + factor_name.

## What You Get

The view provides factor-level readings for each risk component, contribution rankings from 1 to 6 indicating factor importance, and contribution tiers (PRIMARY, SECONDARY, MINOR) for quick filtering. This supports explainability requirements for investment committee presentations and compliance documentation.

## Risk Factors Covered

**Price Stretch** — How extended price is from moving averages.

**Breadth Risk** — Market participation weakness.

**Volatility Pressure** — Volatility stress level.

**Liquidity Stress** — Financial conditions tightness.

**Rate Duration** — Time spent at price extremes.

**Structural Divergence** — Price versus breadth disagreement.

## How Teams Use This

Risk attribution dashboards break down the composite score into its drivers. Investment committee presentations explain why risk changed from last week. Model explainability requirements get satisfied with factor-level documentation. Compliance and audit teams use this for record-keeping. When someone asks "why did risk spike yesterday," this view answers that question.

## Related Views

- Market Regime State — Composite risk state
- Risk Factor Pressure Index — Raw factor readings

## Access

Factor attribution analysis is available to Premium subscribers.

[Request Access](https://altquant.ai/contact)
