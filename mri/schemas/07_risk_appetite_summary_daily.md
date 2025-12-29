# Risk Appetite Summary (Daily)

Composite view of investor risk-seeking versus risk-avoidance behavior across six cross-asset factors. This provides a consolidated view of risk-on versus risk-off conditions with momentum direction for allocation decisions.

## What This Answers

What is the overall risk appetite across markets today?

## Data Structure

One row per trading day. Primary key is market_date.

Refresh is daily after US market close.

Coverage:
- Trial: Last 12 months with 30-day delay
- Core/Premium: Full history from January 2020

## Signal Characteristics

The composite aggregates six underlying factors, which reduces noise compared to any single factor. This is designed for multi-day holding periods rather than intraday trading.

## Who Uses This

Asset allocation models use this as a primary input for equity weight decisions. Risk budget overlays adjust exposure based on composite readings. Systematic macro strategies incorporate this alongside other macro signals. Portfolio construction systems use it for cross-asset positioning.

## Composite States

**RISK_ON** — Markets broadly favor risk-taking. Multiple factors aligned toward growth and beta.

**MIXED** — Selective risk-taking appropriate. Factors are split with no clear consensus.

**RISK_OFF** — Defensive positioning warranted. Multiple factors aligned toward safety.

## Composite Momentum

**IMPROVING** — Risk appetite is increasing across factors.

**STABLE** — No significant directional change in aggregate.

**DETERIORATING** — Risk appetite is decreasing across factors.

## Columns

**market_date** (DATE) — Trading date for summary.

**pairs_risk_on** (INTEGER) — Count of factors currently in RISK_ON state. Range 0-6. Higher indicates broader risk appetite.

**pairs_risk_off** (INTEGER) — Count of factors currently in RISK_OFF state. Range 0-6. Higher indicates broader risk aversion.

**pairs_neutral** (INTEGER) — Count of factors currently in NEUTRAL state. Range 0-6. Many neutral factors suggest an indecisive market.

**composite_state** (VARCHAR) — Overall risk appetite assessment. Valid values: RISK_ON, MIXED, RISK_OFF. This is the primary signal for allocation decisions.

**composite_z_score** (DECIMAL) — Average z-score across all six factors. Positive indicates risk-on bias, negative indicates risk-off bias.

**composite_momentum** (VARCHAR) — Aggregate momentum direction. Valid values: IMPROVING, STABLE, DETERIORATING.

## Working With This Data

The combination of state and momentum tells you more than either alone. RISK_ON with DETERIORATING momentum suggests conditions are fading and may not persist. RISK_OFF with IMPROVING momentum may signal bottom formation before the state flips.

Use this alongside market_regime_state_daily for confirmation. When both views agree, conviction is higher. When they diverge, dig into the underlying factors to understand why.

Multiple factors aligned in the same direction is a stronger signal than a bare majority. Four out of six in RISK_ON is more meaningful than three out of six.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    COMPOSITE_STATE,
    PAIRS_RISK_ON,
    PAIRS_RISK_OFF,
    COMPOSITE_Z_SCORE,
    COMPOSITE_MOMENTUM
FROM ALTQUANT_MRI.CORE.RISK_APPETITE_SUMMARY_DAILY
ORDER BY MARKET_DATE DESC
LIMIT 20;
```

## Related Views

- Risk Appetite Signals — Individual factor details
- Market Regime State — Regime context
- Macro Market Context — Liquidity backdrop
