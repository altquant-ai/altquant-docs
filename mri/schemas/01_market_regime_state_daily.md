# market_regime_state_daily

## Purpose

Core view of the US Equity Market Risk & Regime Signals (MRI) dataset. Daily classification of the market into discrete regime states based on trend strength, volatility conditions, breadth participation, and liquidity stress.

Designed to answer: **"How aggressive should exposure be today?"**

---

## Grain

- **1 row per market_date × index_name**

## Primary Key

- `market_date`
- `index_name`

## Refresh Cadence

- Daily, after US market close

## Coverage

- **Trial**: 36-month rolling (T-30 delay)
- **Core**: Full history from Jan 2020

---

## Column Access

This documentation covers the **Trial/Core** column set (16 columns). Premium subscribers have access to 7 additional diagnostic columns.

---

## Signal Stability

- **High**
- Uses hysteresis to prevent regime whipsaws
- State changes require multi-day confirmation
- Designed for institutional holding periods (days to weeks)

## Typical Consumers

- Systematic equity strategies
- Risk overlays for discretionary PMs
- Position sizing engines
- Drawdown protection systems

---

## Market States (ordered by risk, ascending)

| State | Risk Level | Interpretation |
|-------|------------|----------------|
| BUY_ZONE | 1 (Lowest) | Favorable conditions — full exposure allowed |
| MOMENTUM_OK | 2 | Trend intact — normal exposure appropriate |
| CAUTION | 3 | Mixed signals — reduce exposure, tighten stops |
| HIGH_RISK | 4 (Highest) | Defensive posture — capital preservation priority |

---

## Structural Regimes

| Regime | Interpretation |
|--------|----------------|
| BULL | Sustained uptrend with healthy breadth support |
| NEUTRAL | Transitional or range-bound market conditions |
| BEAR | Sustained downtrend with weak breadth participation |

---

## Confidence Levels

| Level | Interpretation |
|-------|----------------|
| HIGH | Strong conviction in current assessment |
| MEDIUM | Moderate conviction, some mixed signals |
| LOW | Low conviction, conflicting indicators |

---

## Regime Transitions

| Transition | Interpretation |
|------------|----------------|
| ENTERING | Recently transitioned to current state (≤3 days) |
| EXITING | Showing signs of leaving current state |
| PERSISTING | Established trend continuing (>10 days, stable) |
| STABLE | No significant change detected |

---

## Columns

### market_date

- **Type:** DATE
- **Description:** Trading date for which regime is computed
- **Example:** `2025-12-15`

---

### index_name

- **Type:** VARCHAR
- **Description:** Market index identifier (abstracted label)
- **Valid Values:** `Large Cap Core`, `Large Cap Growth`, `Small Cap Core`, `Large Cap Value`, `Broad Market`
- **Example:** `Large Cap Core`

---

### market_state

- **Type:** VARCHAR
- **Description:** Primary regime classification for the day
- **Valid Values:** `BUY_ZONE`, `MOMENTUM_OK`, `CAUTION`, `HIGH_RISK`
- **Interpretation:** Use as a **hard exposure gate** for position management
- **Example:** `MOMENTUM_OK`

---

### structural_regime

- **Type:** VARCHAR
- **Description:** Longer-term market environment classification
- **Valid Values:** `BULL`, `NEUTRAL`, `BEAR`
- **Interpretation:** Provides context for state interpretation; CAUTION in BULL differs from CAUTION in BEAR
- **Example:** `BULL`

---

### regime_days_held

- **Type:** INTEGER
- **Description:** Number of consecutive days current structural regime has persisted
- **Range:** 1+
- **Interpretation:** Higher values indicate regime stability and conviction
- **Example:** `31`

---

### confidence

- **Type:** VARCHAR
- **Description:** Confidence level of regime classification
- **Valid Values:** `HIGH`, `MEDIUM`, `LOW`
- **Interpretation:** Use to scale position sizes or filter signals
- **Example:** `HIGH`

---

### new_longs_allowed

- **Type:** BOOLEAN
- **Description:** Whether conditions favor initiating new long positions
- **Interpretation:** Hard gate for new position entry
- **Example:** `true`

---

### position_size_mult

- **Type:** DECIMAL(3,2)
- **Description:** Recommended position sizing multiplier
- **Range:** 0.00–1.00
- **Interpretation:** 1.0 = full size; 0.5 = half size; 0.0 = no new positions
- **Example:** `0.75`

---

### regime_transition

- **Type:** VARCHAR
- **Description:** Current transition state of the regime
- **Valid Values:** `ENTERING`, `EXITING`, `PERSISTING`, `STABLE`
- **Interpretation:** ENTERING/EXITING suggest higher uncertainty
- **Example:** `PERSISTING`

---

### risk_severity

- **Type:** VARCHAR
- **Description:** Categorical risk severity based on volatility shock conditions
- **Valid Values:** `NORMAL`, `ELEVATED`, `HIGH`, `EXTREME`

| Shock Condition       | risk_severity | Meaning                 |
|-----------------------|---------------|-------------------------|
| none                  | NORMAL        | Standard conditions     |
| term_structure only   | ELEVATED      | VIX curve inverted      |
| vix_velocity only     | HIGH          | Rapid VIX expansion     |
| both                  | EXTREME       | Multiple stress signals |

- **Interpretation:** Use for quick risk bucketing in dashboards and alerts
- **Example:** `NORMAL`

---

### risk_interpretation

- **Type:** VARCHAR
- **Description:** Human-readable summary describing the current market risk environment derived from regime, phase, and severity
- **Valid Values:**
  - `Oversold recovery opportunity` (BUY_ZONE)
  - `Healthy risk-on environment` (MOMENTUM_OK + NORMAL)
  - `Risk-on with elevated volatility` (MOMENTUM_OK + ELEVATED/HIGH)
  - `Risk-on with tail-risk warning` (MOMENTUM_OK + EXTREME)
  - `Market digesting gains` (CAUTION/COMPRESSION + NORMAL)
  - `Market digesting gains (stress rising)` (CAUTION/COMPRESSION + ELEVATED)
  - `Compression with rising stress` (CAUTION/COMPRESSION + HIGH/EXTREME)
  - `Instability risk building` (CAUTION/FRAGILE + NORMAL/ELEVATED)
  - `Elevated tail-risk conditions` (CAUTION/FRAGILE + HIGH/EXTREME)
  - `Defensive but selective conditions` (HIGH_RISK + NORMAL/ELEVATED)
  - `Crisis-level stress` (HIGH_RISK + HIGH/EXTREME)
  - `Transitional conditions` (fallback)
- **Interpretation:** Plain-English market characterization for PM dashboards
- **Example:** `Healthy risk-on environment`

---

### recommended_action

- **Type:** VARCHAR
- **Description:** Guidance enum indicating a suggested risk posture. This is risk posture guidance, not a buy/sell recommendation and not investment advice.
- **Valid Values:**

| Value | Description |
|-------|-------------|
| `FULL_RISK` | Full position sizing, normal operations |
| `HOLD_AND_TIGHTEN` | Hold positions, tighten stops |
| `REDUCE_SLOW` | Gradual risk reduction |
| `REDUCE_FAST` | Accelerated risk reduction |
| `DEFENSIVE_SELECTIVE` | Defensive posture, selective opportunities |

- **Interpretation:** Use as a starting point for portfolio policy decisions
- **Example:** `FULL_RISK`

---

### urgency_level

- **Type:** VARCHAR
- **Description:** Urgency of action based on risk severity
- **Valid Values:**

| Value | Maps From | Interpretation |
|-------|-----------|----------------|
| `LOW` | NORMAL severity | No immediate action required |
| `MEDIUM` | ELEVATED severity | Monitor closely, prepare adjustments |
| `HIGH` | HIGH severity | Act within session |
| `CRITICAL` | EXTREME severity | Immediate attention required |

- **Interpretation:** Use to prioritize attention and response timing
- **Example:** `LOW`

---

### target_exposure_pct

- **Type:** INTEGER
- **Description:** Exposure ceiling derived from regime, severity, and advisory overlays
- **Range:** 0–100 (higher = more exposure allowed)
- **Interpretation:**
  - 100 = Full exposure (MOMENTUM_OK/BUY_ZONE + NORMAL severity)
  - 70 = Fragility cap (MOMENTUM_OK + NORMAL + fragility flag)
  - 35–85 = Severity-adjusted (depends on regime × severity)
  - 10–25 = HIGH_RISK regime
- **Example:** `85`

---

### exposure_band

- **Type:** VARCHAR
- **Description:** Human-readable exposure bucket derived from target_exposure_pct
- **Valid Values:** `Full`, `Moderate`, `Reduced`, `Minimal`

| Band | Target Exposure | Interpretation |
|------|-----------------|----------------|
| Full | ≥90% | Normal operations, full position sizing |
| Moderate | 60–89% | Some caution, moderate sizing |
| Reduced | 30–59% | Defensive posture, reduced sizing |
| Minimal | <30% | Capital preservation priority |

- **Example:** `Moderate`

---

### positioning_bias

- **Type:** VARCHAR
- **Description:** High-level posture derived from target_exposure_pct
- **Valid Values:** `Risk-on`, `Cautious`, `Defensive`, `Capital preservation`

| Bias | Target Exposure | Interpretation |
|------|-----------------|----------------|
| Risk-on | ≥90% | Favorable conditions for risk-taking |
| Cautious | 60–89% | Proceed with caution |
| Defensive | 30–59% | Prioritize capital protection |
| Capital preservation | <30% | Minimize exposure |

- **Example:** `Cautious`

---

## Premium Columns (not included)

The following diagnostic columns are available with Premium subscription:

| Column | Description |
|--------|-------------|
| `volatility_z_20d` | 20-day volatility z-score |
| `signal_alignment` | Count of agreeing risk components |
| `elevated_driver_count` | Elevated risk factor count |
| `regime_stability` | Categorical stability assessment |
| `caution_phase` | CAUTION sub-classification |
| `structural_fragility_flag` | Advisory fragility flag |
| `structural_fragility_reason` | Fragility explanation |

---

## Usage Notes

- Regime changes are **stateful** — hysteresis is applied to prevent whipsaws
- HIGH_RISK states are often driven by volatility or liquidity overrides
- Combine with `market_participation_daily` for breadth confirmation
- Use `regime_days_held` to assess regime maturity before acting
- `position_size_mult` is a suggestion — integrate with your risk framework

---

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    INDEX_NAME,
    MARKET_STATE,
    STRUCTURAL_REGIME,
    CONFIDENCE,
    REGIME_DAYS_HELD,
    POSITION_SIZE_MULT,
    TARGET_EXPOSURE_PCT
FROM ALTQUANT_MRI.CORE.MARKET_REGIME_STATE_DAILY
WHERE INDEX_NAME = 'Large Cap Core'
ORDER BY MARKET_DATE DESC
LIMIT 20;
```

---

## Related Views

- market_participation_daily — Breadth confirmation
- risk_appetite_summary_daily — Cross-asset risk appetite
- market_stress_flags_daily — Binary stress indicators
