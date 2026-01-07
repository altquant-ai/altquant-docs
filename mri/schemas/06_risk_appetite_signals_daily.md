# Risk Appetite Signals (Daily)

Individual factor readings measuring investor preference for risk across equity beta, credit, cyclical/defensive, size, and style dimensions. This enables factor-level analysis of where risk appetite is strongest or weakest.

## What This Answers

Is money flowing toward or away from risk?

## Data Structure

One row per trading day per risk factor. Primary key is market_date + risk_factor_code.

Refresh is daily after US market close.

Coverage:
- Trial: 36-month rolling (T-30 delay)
- Core/Premium: Full history from Jan 2020

## Signal Characteristics

Factor ratios use rolling windows for stability. State changes require sustained moves rather than single-day spikes. This makes the signals appropriate for allocation decisions over multi-day horizons.

## Who Uses This

Risk-on/risk-off allocation models use these factors as inputs. Factor rotation strategies monitor for shifts in risk appetite. Cross-asset confirmation strategies check whether equity signals align with credit signals. Macro overlay systems incorporate these readings.

## Risk Factors

**ERS (Equity Risk Sensitivity)** — Measures preference for high-beta versus low-volatility stocks.

**CRP (Credit Risk Preference)** — Measures appetite for credit risk through high yield versus safe haven spreads.

**CDS (Cyclical-Defensive Sector)** — Measures relative strength of cyclical versus defensive sectors.

**CDS-2 (Cyclical-Defensive Sector 2)** — Alternative cyclical/defensive measurement using different sector groupings.

**SRP (Size Risk Preference)** — Measures small cap versus large cap relative performance.

**STP (Style Risk Preference)** — Measures consumer discretionary versus staples preference.

## Factor States

**RISK_ON** — Risk appetite elevated. Market favors growth and beta.

**NEUTRAL** — Balanced risk appetite. No strong directional preference.

**RISK_OFF** — Risk aversion dominant. Market favors safety and quality.

## Factor Momentum

**STRENGTHENING** — Factor trending toward more risk-on conditions.

**STABLE** — No significant directional change.

**WEAKENING** — Factor trending toward more risk-off conditions.

## Columns

**market_date** (DATE) — Trading date for factor calculation.

**risk_factor_name** (VARCHAR) — Human-readable factor name.

**risk_factor_code** (VARCHAR) — Short identifier. Valid values: ERS, CRP, CDS, CDS-2, SRP, STP.

**factor_category** (VARCHAR) — Classification of factor type. Valid values: equity_beta, credit, cyclical_defensive, size, style.

**z_score** (DECIMAL) — Standardized factor reading. Positive indicates risk-on bias, negative indicates risk-off bias.

**factor_state** (VARCHAR) — Current state classification. Valid values: RISK_ON, NEUTRAL, RISK_OFF.

**factor_momentum** (VARCHAR) — 20-day directional momentum. Valid values: STRENGTHENING, STABLE, WEAKENING.

## Working With This Data

Count factors in each state for an aggregate view. The risk_appetite_summary_daily view does this for you if you just need the composite.

Watch for divergence between factors. When credit factors diverge from equity factors, that often signals a regime transition in progress.

Credit factors (CRP) tend to lead equity factors. Pay attention when credit is weakening while equity is still showing risk-on.

Factor momentum helps distinguish trend setups from mean-reversion setups. A factor at extreme levels with stable momentum suggests continuation. The same factor with reversing momentum suggests mean-reversion.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    RISK_FACTOR_CODE,
    RISK_FACTOR_NAME,
    Z_SCORE,
    FACTOR_STATE,
    FACTOR_MOMENTUM
FROM ALTQUANT_MRI.PREMIUM.RISK_APPETITE_SIGNALS_DAILY
WHERE MARKET_DATE = (SELECT MAX(MARKET_DATE) FROM ALTQUANT_MRI.PREMIUM.RISK_APPETITE_SIGNALS_DAILY)
ORDER BY ABS(Z_SCORE) DESC;
```

## Related Views

- Risk Appetite Summary — Aggregated summary across all factors
- Market Regime State — Regime context
- Macro Market Context — Liquidity conditions
