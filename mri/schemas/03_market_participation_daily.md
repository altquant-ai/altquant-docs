# Market Participation Metrics (Daily)

Breadth indicators measuring how broadly the market is participating in price moves. This includes advancing/declining analysis, intermediate-term breadth health, and breadth thrust detection for momentum confirmation.

## What This Answers

Is this rally or decline broad-based or narrow?

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

Refresh is daily after US market close.

Coverage:
- Trial: Last 12 months with 30-day delay
- Core/Premium: Full history from January 2020

## Signal Characteristics

Daily breadth readings can be noisy on any given day. The intermediate-term metrics are more stable and reliable for trend assessment. Thrust signals use multi-day confirmation before triggering.

## Who Uses This

Trend-following strategies use breadth as a confirmation layer. Risk overlays check whether price moves have broad support. Rally and correction assessment relies on participation metrics. Mean-reversion timing strategies watch for breadth extremes.

## Breadth Assessments

**Strong** — Excellent participation. The market has broad support.

**Positive** — Good breadth. Healthy rally or trend conditions.

**Neutral** — Mixed participation. Proceed with selectivity.

**Negative** — Deteriorating breadth. Leadership is narrowing.

**Weak** — Poor participation. Elevated risk of reversal.

## Thrust Events

**THRUST_ON** — Breadth thrust signal initiated. Historically bullish.

**THRUST_CONFIRM** — Thrust signal confirmed. Strong bullish indication.

**THRUST_FAIL** — Thrust signal failed. Bullish case weakened.

**NONE** — No active thrust signal.

## Columns

**market_date** (DATE) — Trading date for breadth calculation.

**index_name** (VARCHAR) — Market index identifier. Valid values: Large Cap Core, Large Cap Growth, Small Cap Core, Large Cap Value, Broad Market.

**daily_breadth_pct** (DECIMAL) — Percentage of stocks advancing on the day. Higher indicates stronger buying pressure.

**intermediate_breadth_pct** (DECIMAL) — Percentage of stocks above their 50-day moving average. This is the key indicator of intermediate-term trend health.

**long_term_breadth_pct** (DECIMAL) — Percentage of stocks above their 200-day moving average. Indicates bull versus bear market structure.

**breadth_momentum** (DECIMAL) — Short-term breadth momentum oscillator. Positive values indicate bullish momentum, negative indicates bearish.

**breadth_assessment** (VARCHAR) — Composite breadth evaluation. Valid values: Strong, Positive, Neutral, Negative, Weak.

**thrust_signal_active** (BOOLEAN) — Whether a breadth thrust signal is currently active.

**thrust_event** (VARCHAR) — Current thrust signal state. Valid values: THRUST_ON, THRUST_CONFIRM, THRUST_FAIL, NONE.

**thrust_strength** (DECIMAL) — Normalized thrust signal strength. Higher values indicate stronger signals.

## Working With This Data

Combine daily and intermediate breadth for a complete picture. Daily can whipsaw, but when both daily and intermediate agree, the signal is more reliable.

Thrust signals are rare. When they occur, they're historically significant. Don't expect to see them often.

Watch for breadth divergences where price moves up but breadth moves down. These are warning signs that a rally may be losing steam.

Use breadth_assessment for quick filtering in screens when you need a single categorical value.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    INDEX_NAME,
    DAILY_BREADTH_PCT,
    INTERMEDIATE_BREADTH_PCT,
    BREADTH_ASSESSMENT,
    THRUST_SIGNAL_ACTIVE
FROM ALTQUANT_MRI.CORE.MARKET_PARTICIPATION_DAILY
WHERE INDEX_NAME = 'Large Cap Core'
ORDER BY MARKET_DATE DESC
LIMIT 10;
```

## Related Views

- Market Regime State — Regime context
- Market Breadth Structure — Phase analysis
- Trend Alignment Index — Divergence detection
