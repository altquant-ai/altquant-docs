# How to Read MRI Signals

The MRI dataset exposes interpretable states, not raw model mechanics. The indicators are designed to be read together, following a simple decision flow:

**Environment → Risk → Confirmation → Action**

---

## Quick Start

1. **Start with `MARKET_STATE`** — What environment am I in?
2. **Check `RISK_SEVERITY` + `URGENCY_LEVEL`** — How urgent is this?
3. **Confirm with `BREADTH_ASSESSMENT` + `COMPOSITE_STATE`** — Is the market aligned internally?
4. **Act using `TARGET_EXPOSURE_PCT` and `RECOMMENDED_ACTION`** — How much risk is allowed?

---

## Step 1: Identify the Market Environment

### MARKET_STATE

Defines the current tactical environment.

| State | Interpretation |
|-------|----------------|
| `BUY_ZONE` | Rare. Exceptional conditions where risk, participation, and trend are unusually aligned |
| `MOMENTUM_OK` | Trend is intact, risk-taking is permitted |
| `CAUTION` | Market is transitioning; risk is rising but not broken |
| `HIGH_RISK` | Defensive regime; drawdown risk elevated |

This is the starting point for interpretation.

> **Design Note:** `BUY_ZONE` is intentionally rare. The model is calibrated as risk infrastructure, not an alpha or entry signal. Frequent "buy" states would encourage false optimism and over-trading. When `BUY_ZONE` appears, it reflects broad-based favorable conditions across multiple dimensions — not a trading recommendation.

### STRUCTURAL_REGIME

Provides the longer-horizon backdrop.

| Regime | Interpretation |
|--------|----------------|
| `BULL` | Long-term uptrend |
| `NEUTRAL` | Range-bound / unclear trend |
| `BEAR` | Long-term downtrend |

> **Important:** A `HIGH_RISK` state can occur inside a `BULL` regime. This signals tactical danger, not a full cycle reversal.

---

## Step 2: Assess Stability and Transition Risk

### REGIME_TRANSITION

Describes how the regime is changing.

| Transition | Interpretation |
|------------|----------------|
| `ENTERING` | New regime just triggered (unstable) |
| `STABLE` | No recent transition |
| `PERSISTING` | Regime continuing as expected |
| `EXITING` | Regime losing validity |

Use this to judge confidence and whipsaw risk.

### CONFIDENCE

Model confidence in the current regime.

| Level | Interpretation |
|-------|----------------|
| `HIGH` | Strong internal agreement |
| `MEDIUM` | Mixed signals |
| `LOW` | Fragile, higher uncertainty |

> **Key insight:** Low confidence + recent transition = higher caution, even if risk appears benign.

### CAUTION_PHASE

Only populated when MARKET_STATE = CAUTION.

| Phase | Interpretation |
|-------|----------------|
| `CONSOLIDATION` | Controlled pullback, healthy digestion |
| `INSTABILITY` | Participation breaking down, risk escalating |

This tells you whether `CAUTION` is benign or deteriorating.

### STRUCTURAL_FRAGILITY_FLAG

A boolean advisory flag that fires when the regime appears risk-on, but internal conditions are weakening.

| Value | Interpretation |
|-------|----------------|
| `TRUE` | Trend intact but internals deteriorating — tighten risk |
| `FALSE` | No fragility detected |

> **Key insight:** Structural fragility highlights situations where the trend remains intact, but internal participation has weakened — a condition historically associated with asymmetric downside. When this flag is `TRUE`, consider reducing position sizes even if `MARKET_STATE` is permissive.

---

## Step 3: Evaluate Stress and Urgency

### RISK_SEVERITY

Measures shock intensity, not direction.

| Severity | Interpretation |
|----------|----------------|
| `NORMAL` | No stress |
| `ELEVATED` | Early warning |
| `HIGH` | Active stress |
| `EXTREME` | Shock conditions (volatility/liquidity) |

> **Key rule:** Severity controls urgency, not trend bias.

### URGENCY_LEVEL

Translates severity into timing pressure.

| Level | Interpretation |
|-------|----------------|
| `LOW` | No immediate action needed |
| `MEDIUM` | Prepare / adjust |
| `HIGH` | Action required |
| `CRITICAL` | Immediate risk reduction |

---

## Step 4: Confirm with Internal Participation

### BREADTH_ASSESSMENT

Shows how widely the market move is supported.

| Assessment | Interpretation |
|------------|----------------|
| `Strong` | Broad participation, healthy internals |
| `Positive` | Above-average participation |
| `Neutral` | Mixed participation |
| `Negative` | Below-average participation |
| `Weak` | Narrow leadership, fragile trend |

> Weak breadth during `MOMENTUM_OK` is a warning; weak breadth during `HIGH_RISK` confirms defense.

### COMPOSITE_STATE (Risk Appetite)

Cross-asset confirmation from 6 risk factors.

| State | Interpretation |
|-------|----------------|
| `RISK_ON` | Risk assets favored across factors |
| `MIXED` | Conflicting signals |
| `RISK_OFF` | Defensive assets favored |

Risk-off appetite reinforces defensive regimes.

### THRUST_EVENT

Participation acceleration signals.

| Event | Interpretation |
|-------|----------------|
| `THRUST_ON` | Breadth thrust initiated |
| `THRUST_CONFIRM` | Thrust confirmed (historically bullish) |
| `THRUST_FAIL` | Bullish attempt failed |
| `NONE` | No thrust active |

> Thrusts are confirmations, not standalone signals.

---

## Step 5: Act Using Guidance Outputs

### RECOMMENDED_ACTION

What the model suggests doing now.

| Action | Interpretation |
|--------|----------------|
| `FULL_RISK` | Normal risk deployment |
| `HOLD_AND_TIGHTEN` | Maintain exposure, reduce risk tolerance |
| `REDUCE_SLOW` | Gradual de-risking |
| `REDUCE_FAST` | Urgent risk reduction |
| `DEFENSIVE_SELECTIVE` | Only high-quality, low-beta exposure |

### EXPOSURE_BAND

How much risk is allowed.

| Band | Target Exposure | Interpretation |
|------|-----------------|----------------|
| `Full` | ≥90% | Normal exposure |
| `Moderate` | 60–89% | Reduced exposure |
| `Reduced` | 30–59% | Defensive posture |
| `Minimal` | <30% | Capital preservation mode |

### POSITIONING_BIAS

Overall posture summary.

| Bias | Interpretation |
|------|----------------|
| `Risk-on` | Favorable conditions for risk-taking |
| `Cautious` | Proceed with caution |
| `Defensive` | Prioritize capital protection |
| `Capital preservation` | Minimize exposure |

---

## How to Combine Signals

**Mental Model:**

1. **Start with MARKET_STATE** → What environment am I in?
2. **Check RISK_SEVERITY + URGENCY** → How urgent is this?
3. **Confirm with BREADTH + COMPOSITE_STATE** → Is the market aligned internally?
4. **Follow RECOMMENDED_ACTION + EXPOSURE_BAND** → How much risk is allowed?

---

## Example Interpretations

### Defensive Scenario
```
HIGH_RISK + EXTREME + RISK_OFF + Weak Breadth
```
→ Defensive regime confirmed
→ `REDUCE_FAST`, Minimal exposure, Capital preservation

### Risk-On Scenario
```
MOMENTUM_OK + NORMAL + Strong Breadth + RISK_ON
```
→ Healthy risk-on environment
→ `FULL_RISK`, Full exposure

### Transitional Scenario
```
CAUTION + INSTABILITY + ELEVATED + Negative Breadth + MIXED
```
→ Deteriorating conditions, not yet crisis
→ `REDUCE_SLOW`, Moderate exposure, Cautious

### Rare Favorable Scenario
```
BUY_ZONE + HIGH Confidence + Improving Momentum + THRUST_ON
```
→ Exceptional alignment across risk, participation, and trend
→ Full exposure permitted; not an entry signal but a permissive environment

---

## Quick Reference

| Signal Category | Primary Field | Confirms With |
|-----------------|---------------|---------------|
| Environment | `MARKET_STATE` | `STRUCTURAL_REGIME` |
| Stability | `REGIME_TRANSITION` | `CONFIDENCE`, `CAUTION_PHASE` |
| Stress | `RISK_SEVERITY` | `URGENCY_LEVEL` |
| Participation | `BREADTH_ASSESSMENT` | `COMPOSITE_STATE`, `THRUST_EVENT` |
| Action | `RECOMMENDED_ACTION` | `EXPOSURE_BAND`, `POSITIONING_BIAS` |

---

## Important Note

These enums are not trading signals. They form a risk framework designed to keep exposure aligned with market conditions and avoid emotional decision-making.

The model is calibrated as risk infrastructure — it tells you how much risk is appropriate, not what to buy. States like `BUY_ZONE` are intentionally rare to prevent false optimism. The framework's value is in risk reduction and regime awareness, not alpha generation.
