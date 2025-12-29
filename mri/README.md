# MRI - Market Risk Intelligence

US Equity Market Risk & Regime Signals (Daily) provides a structured, systematic view of equity market conditions designed for institutional research and quantitative workflows. The dataset delivers daily indicators covering market risk regimes, participation and breadth dynamics, volatility and liquidity conditions, trend structure, macro context, and stress flags across major US equity universes. Outputs are deterministic, point-in-time, and engineered for direct integration into portfolio construction, risk management, research pipelines, and AI-native analytics systems. The dataset covers 5 US equity indices with daily updates after market close, and has been tested across every major market event since January 2020.

This product is intended to operate upstream of securities selection and portfolio decisions, providing consistent market context rather than security-level forecasts or trade recommendations.

---

## Key Coverage Areas

| Area | Description |
|------|-------------|
| Market Risk Regimes | Systematic classification of volatility and trend states |
| Breadth & Participation | Real-time assessment of market internals beyond price |
| Structural Stress | Early-warning flags for liquidity and volatility dislocations |
| Macro Context | Alignment between equity behavior and broader macro/rate environments |

---

## Market Conditions Tested

MRI has navigated the following market environments:

| Period | Event | Conditions |
|--------|-------|------------|
| Feb-Mar 2020 | COVID-19 Crash | 34% drawdown, VIX spike to 82 |
| Nov 2020 | Post-Election Rally | Regime transition, sector rotation |
| Jan 2021 | Meme Stock Volatility | Narrow leadership, breadth divergence |
| 2022 | Bear Market | Sustained downtrend, rate shock |
| Mar 2023 | Banking Crisis | Liquidity stress, flight to safety |
| Oct 2023 | Q4 Correction | VIX spike, breadth collapse |
| 2024 | Recovery and Rotation | Mixed regimes, sector dispersion |
| Mar-Apr 2025 | Tariff Volatility | Trade policy uncertainty, sector stress |

---

## What MRI Provides

- **Market Regime Classification** — Daily state assessment (BUY_ZONE, MOMENTUM_OK, CAUTION, HIGH_RISK)
- **Risk Severity and Urgency** — Shock intensity and timing pressure indicators
- **Breadth and Participation Metrics** — Market internals and thrust signals
- **Cross-Asset Risk Appetite** — Risk-on/risk-off readings across 6 factors
- **Stress Flags** — Binary indicators for volatility spikes, liquidity stress, and breadth collapse
- **Exposure Guidance** — Target exposure bands and positioning bias

---

## What MRI Does NOT Do

| Does Not | Explanation |
|----------|-------------|
| Predict returns | No forecast of price direction or magnitude |
| Provide entry/exit signals | Not a timing or alpha model |
| Optimize portfolios | No security selection or weighting |
| Replace strategy logic | Framework for risk sizing, not strategy |

---

## Who Uses MRI

- **Systematic Strategies** — Risk overlay for position sizing and exposure management
- **Discretionary Managers** — Market environment context for tactical decisions
- **Research Teams** — Regime analysis and historical backtesting
- **Risk Management** — Stress monitoring and early warning signals

---

## Index Coverage

| Index | Description |
|-------|-------------|
| Large Cap Core | Large cap broad market |
| Large Cap Growth | Large cap growth stocks |
| Large Cap Value | Large cap value stocks |
| Small Cap Core | Small cap broad market |
| Broad Market | Total market coverage |

---

## Subscription Tiers and Views

| View | Trial | Core | Premium |
|------|:-----:|:----:|:-------:|
| **Core Views** | | | |
| [Market Regime State](schemas/01_market_regime_state_daily.md) | ✓ | ✓ | ✓ (all columns) |
| [Market Stress Flags](schemas/02_market_stress_flags_daily.md) | ✓ | ✓ | ✓ |
| [Market Participation](schemas/03_market_participation_daily.md) | ✓ | ✓ | ✓ |
| [Risk Appetite Summary](schemas/07_risk_appetite_summary_daily.md) | ✓ | ✓ | ✓ |
| [Macro Context](schemas/10_macro_context_daily.md) | ✓ | ✓ | ✓ |
| [Data Quality Summary](schemas/12_data_quality_summary_daily.md) | ✓ | ✓ | ✓ |
| [Market Index OHLC](schemas/market_index_ohlc_daily.md) | ✓ | ✓ | ✓ |
| **Premium Views** | | | |
| [Market Breadth Structure](schemas/04_market_breadth_structure_daily.md) | | | ✓ |
| [Trend Convergence](schemas/05_trend_convergence_daily.md) | | | ✓ |
| [Risk Appetite Signals](schemas/06_risk_appetite_signals_daily.md) | | | ✓ |
| [Risk Driver Attribution](schemas/08_risk_driver_contributions_daily.md) | | | ✓ |
| [Risk Factor Z-Scores](schemas/09_risk_factor_zscores_daily.md) | | | ✓ |
| [AI Market Narrative](schemas/11_market_regime_quantgpt_daily.md) | | | ✓ |
| | | | |
| **History** | 12 months | Jan 2020 - Present | Jan 2020 - Present |
| **Refresh** | 30-day delay | After market close | After market close |
| **Total Views** | 7 | 7 | 13 |

Market Regime State has column-gating: Trial and Core receive core columns, Premium receives all columns.

---

## Snowflake Schema Reference

In Snowflake, views are organized by subscription tier:

| Tier | Schema Path |
|------|-------------|
| Trial | `ALTQUANT_MRI.TRIAL.<view_name>` |
| Core | `ALTQUANT_MRI.CORE.<view_name>` |
| Premium | `ALTQUANT_MRI.PREMIUM.<view_name>` |

Example (Core tier):

```sql
SELECT * FROM ALTQUANT_MRI.CORE.MARKET_REGIME_STATE_DAILY
WHERE INDEX_NAME = 'Large Cap Core'
ORDER BY MARKET_DATE DESC
LIMIT 10;
```

---

## Data Access

MRI data is available through:

- **Snowflake Marketplace**
- **AWS Data Exchange**

---

## Documentation

- [Interpretation Guide](interpretation_guide.md)
- [Schema Diagram](schema_diagram.md)
- [Availability & Tiers](availability.md)
- [Compliance & Disclaimers](compliance.md)

---

## Contact

For access requests or questions: [altquant.ai/contact](https://altquant.ai/contact)
