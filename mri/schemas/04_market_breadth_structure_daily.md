# Market Breadth Structure (Daily)

Structural analysis of market breadth to detect accumulation, distribution, and exhaustion phases. This view identifies when rallies are extending beyond sustainable participation levels.

## What This Answers

Is the market in accumulation, distribution, or exhaustion?

## Availability

This view is available in Core and Premium tiers only.

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

## What You Get

The view provides breadth phase classification (ACCUMULATION, DISTRIBUTION, EXHAUSTION, NEUTRAL), high/low ratio analysis comparing new 52-week highs versus lows, volume participation metrics tracking advancing versus declining volume, advance-decline trend assessment, and thrust signal analysis including strength and maturity tracking.

## Breadth Phases

**ACCUMULATION** — Broad buying pressure across the market. Conditions are favorable for longs.

**DISTRIBUTION** — Broad selling pressure. Defensive posture makes sense.

**EXHAUSTION** — Rally has extended and breadth is fatiguing. Caution warranted.

**NEUTRAL** — No dominant phase detected.

## How Teams Use This

Market timing overlays rely on phase detection to adjust exposure. Strategy selection can be phase-aware, running different playbooks in accumulation versus distribution. The exhaustion detection serves as an early warning system before breadth fully breaks down.

## Related Views

- Market Participation Metrics — Raw breadth metrics
- Trend Alignment Index — Divergence analysis
- Market Stress Indicators — Stress indicators

## Access

Breadth structure analysis is available to Core and Premium subscribers.

[Request Access](https://altquant.ai/contact)
