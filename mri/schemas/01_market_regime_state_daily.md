# Market Regime State (Daily)

Daily classification of market conditions into interpretable states. This view combines trend, volatility, breadth, and liquidity into a unified regime framework for risk monitoring and research overlays.

Updated after US market close each trading day.

## What This Answers

What is the current market risk environment?

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

Refresh is daily after US market close.

Coverage varies by tier:
- Trial: Last 12 months with 30-day delay
- Core: 6 years historical plus daily updates
- Premium: Full history from January 2020

## Signal Characteristics

The regime classification is designed to be stable. State changes require multi-day confirmation through hysteresis logic, which prevents whipsaws during choppy markets. This makes the signal appropriate for institutional holding periods measured in days to weeks rather than intraday trading.

## Who Uses This

Systematic equity strategies use this as a primary risk overlay. Discretionary portfolio managers use it for position sizing decisions. Risk teams use it for drawdown protection frameworks.

## Market States

States are ordered by risk level from lowest to highest:

**BUY_ZONE** — Favorable conditions. Historically more supportive for risk-taking.

**MOMENTUM_OK** — Trend intact. Normal operating conditions.

**CAUTION** — Mixed signals. Elevated risk warrants selectivity.

**HIGH_RISK** — Elevated risk. Historically challenging environment for longs.

## Structural Regimes

These provide longer-term context for interpreting the daily state:

**BULL** — Sustained uptrend with healthy breadth support.

**NEUTRAL** — Transitional or range-bound conditions.

**BEAR** — Sustained downtrend with weak breadth participation.

## Confidence Levels

**HIGH** — Strong conviction. Multiple signals aligned.

**MEDIUM** — Moderate conviction. Some mixed signals present.

**LOW** — Low conviction. Conflicting indicators.

## Regime Transitions

**ENTERING** — Recently transitioned to current state.

**EXITING** — Showing signs of leaving current state.

**PERSISTING** — Established trend continuing.

**STABLE** — No significant change detected.

## Columns

**market_date** (DATE) — Trading date for which regime is computed.

**index_name** (VARCHAR) — Market index identifier. Valid values: Large Cap Core, Large Cap Growth, Small Cap Core, Large Cap Value, Broad Market.

**market_state** (VARCHAR) — Primary regime classification. Valid values: BUY_ZONE, MOMENTUM_OK, CAUTION, HIGH_RISK.

**structural_regime** (VARCHAR) — Longer-term market environment. Valid values: BULL, NEUTRAL, BEAR.

**regime_days_held** (INTEGER) — Consecutive days the current structural regime has persisted. Higher values indicate stability and conviction.

**signal_alignment** (INTEGER) — Count of risk components agreeing on current direction. Higher means more conviction.

**confidence** (VARCHAR) — Confidence level of regime classification. Valid values: HIGH, MEDIUM, LOW.

**new_longs_allowed** (BOOLEAN) — Whether conditions favor initiating new long positions. Informational flag for policy frameworks.

**position_size_mult** (DECIMAL) — Risk-adjusted sizing factor derived from regime conditions. Normalized scale where 1.0 is baseline.

**regime_stability** (VARCHAR) — Categorical stability assessment. Valid values: HIGH, MODERATE, LOW.

**elevated_driver_count** (INTEGER) — Number of risk factors showing elevated readings. Higher count indicates more stress.

**regime_transition** (VARCHAR) — Current transition state. Valid values: ENTERING, EXITING, PERSISTING, STABLE.

**risk_severity** (VARCHAR) — Categorical risk severity. Valid values: NORMAL, ELEVATED, HIGH, EXTREME.

**caution_phase** (VARCHAR) — Sub-classification when market_state is CAUTION. Valid values: CONSOLIDATION, INSTABILITY, or NULL.

**risk_interpretation** (VARCHAR) — Human-readable summary of current risk environment. Examples: "Healthy risk-on environment", "Market digesting gains", "Crisis-level stress".

**recommended_action** (VARCHAR) — Risk posture guidance, not a buy/sell recommendation. Valid values: FULL_RISK, HOLD_AND_TIGHTEN, REDUCE_SLOW, REDUCE_FAST, DEFENSIVE_SELECTIVE.

**urgency_level** (VARCHAR) — Urgency based on risk severity. Valid values: LOW, MEDIUM, HIGH, CRITICAL.

**structural_fragility_flag** (BOOLEAN) — Advisory flag indicating risk-on conditions with internal fragility.

**structural_fragility_reason** (VARCHAR) — Human-readable reason when fragility flag is TRUE. NULL otherwise.

**target_exposure_pct** (INTEGER) — Exposure metric derived from regime and severity. Range 0-100 where higher indicates more favorable conditions.

**exposure_band** (VARCHAR) — Human-readable exposure bucket. Valid values: Full, Moderate, Reduced, Minimal.

**positioning_bias** (VARCHAR) — High-level risk posture. Valid values: Risk-on, Cautious, Defensive, Capital preservation.

## Working With This Data

Regime changes are stateful. The hysteresis logic means you won't see rapid back-and-forth transitions even in volatile markets.

HIGH_RISK states are often driven by volatility or liquidity conditions overriding other factors. When you see HIGH_RISK with NORMAL severity, that's telling you something specific: structural concerns without acute panic.

Use regime_days_held to assess whether a regime is mature before drawing strong conclusions. A regime that's persisted for 30+ days carries different weight than one that just started.

Combine this with market_participation_daily for breadth confirmation when you want to validate whether the regime classification matches underlying market internals.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    INDEX_NAME,
    MARKET_STATE,
    CONFIDENCE,
    REGIME_DAYS_HELD
FROM ALTQUANT_MRI.CORE.MARKET_REGIME_STATE_DAILY
WHERE INDEX_NAME = 'Large Cap Core'
ORDER BY MARKET_DATE DESC
LIMIT 10;
```

## Related Views

- Market Participation Metrics — Breadth confirmation
- Risk Appetite Summary — Cross-asset risk appetite
- Market Stress Indicators — Binary stress indicators

## Disclaimer

This is informational regime classification for research and monitoring. It does not constitute investment advice.
