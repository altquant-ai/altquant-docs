# Market Stress Indicators (Daily)

Binary and categorical indicators of elevated market stress. This includes volatility shocks, liquidity stress, breadth breakdowns, and structural divergences. Designed for alert generation and research analysis.

## What This Answers

Is there acute market stress that warrants additional attention?

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

Refresh is daily after US market close.

Coverage:
- Trial: 36-month rolling (T-30 delay)
- Core/Premium: Full history from Jan 2020

## Signal Characteristics

These are binary flags with defined thresholds. They're designed for alert and monitoring systems where you need clear yes/no signals rather than continuous scores.

## Who Uses This

Risk override systems use these flags to trigger defensive actions. Position sizing engines scale down when flags are active. Drawdown protection frameworks monitor for flag activation. Alert and monitoring dashboards surface these for human review.

## Stress Flags

**volatility_shock_flag** — Detects sudden, statistically significant volatility spikes.

**volatility_curve_inversion** — Detects when near-term fear exceeds long-term expectations.

**liquidity_stress_flag** — Detects systemic financial conditions tightening.

**participation_breakdown_flag** — Detects rapid deterioration in market breadth.

**structural_divergence_flag** — Detects price/breadth divergence combined with stress conditions.

**volatility_shock_detected** — Composite volatility shock indicator.

**risk_severity** — Categorical risk severity from NORMAL through EXTREME.

## Columns

**market_date** (DATE) — Trading date for stress assessment.

**index_name** (VARCHAR) — Market index identifier. Valid values: Large Cap Core, Large Cap Growth, Small Cap Core, Large Cap Value, Broad Market.

**volatility_shock_flag** (BOOLEAN) — TRUE when a sudden volatility spike is detected. Indicates heightened short-term risk conditions.

**volatility_curve_inversion** (BOOLEAN) — TRUE when volatility term structure is inverted (near-term exceeds long-term). Often precedes corrections.

**liquidity_stress_flag** (BOOLEAN) — TRUE when systemic liquidity measures indicate stress. Signals credit or funding stress.

**participation_breakdown_flag** (BOOLEAN) — TRUE when market internals are collapsing rapidly.

**structural_divergence_flag** (BOOLEAN) — TRUE when multiple stress indicators align with bearish divergence. Indicates elevated probability of correction.

**any_stress_flag_active** (BOOLEAN) — Convenience field. TRUE if any of the above flags is active. Use for quick screening.

**risk_severity** (VARCHAR) — Categorical risk severity. Valid values: NORMAL, ELEVATED, HIGH, EXTREME. Consistent with market_regime_state_daily.

**volatility_shock_detected** (BOOLEAN) — Composite volatility shock indicator.

**risk_interpretation** (VARCHAR) — Human-readable summary of current stress conditions. Examples: "Normal stress conditions", "Multiple stress signals active".

**recommended_action** (VARCHAR) — Risk posture guidance as a stress overlay. This is not a buy/sell recommendation. Valid values: HOLD_AND_TIGHTEN, REDUCE_SLOW, REDUCE_FAST.

**urgency_level** (VARCHAR) — Urgency based on stress severity. Valid values: LOW, MEDIUM, HIGH, CRITICAL.

## Working With This Data

Any TRUE flag suggests elevated risk and warrants additional attention. Multiple TRUE flags indicate a significant stress pattern that has historically been meaningful.

Use any_stress_flag_active for quick portfolio-wide screening when you need to know if anything is elevated without checking each flag individually.

These flags are designed to be rare but meaningful. If they fired constantly, they wouldn't be useful. When they do fire, pay attention.

Combine with regime state for context. A stress flag during a HIGH_RISK regime means something different than the same flag during BUY_ZONE conditions.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    INDEX_NAME,
    ANY_STRESS_FLAG_ACTIVE,
    VOLATILITY_SHOCK_FLAG,
    LIQUIDITY_STRESS_FLAG
FROM ALTQUANT_MRI.CORE.MARKET_STRESS_FLAGS_DAILY
WHERE ANY_STRESS_FLAG_ACTIVE = TRUE
ORDER BY MARKET_DATE DESC;
```

## Related Views

- Market Regime State — Regime context
- Risk Appetite Summary — Risk appetite
- Macro Market Context — Liquidity backdrop

## Disclaimer

This is informational stress classification for research and monitoring. It does not constitute investment advice.
