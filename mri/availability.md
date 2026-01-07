# MRI Data Availability

Coverage and history by subscription tier.

This product is intended to operate upstream of securities selection and portfolio decisions, providing consistent market context rather than security-level forecasts or trade recommendations.

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
| [Market Index OHLC](schemas/13_market_index_ohlc_daily.md) | ✓ | ✓ | ✓ |
| **Premium Views** | | | |
| [Market Breadth Structure](schemas/04_market_breadth_structure_daily.md) | | | ✓ |
| [Trend Convergence](schemas/05_trend_convergence_daily.md) | | | ✓ |
| [Risk Appetite Signals](schemas/06_risk_appetite_signals_daily.md) | | | ✓ |
| [Risk Driver Attribution](schemas/08_risk_driver_contributions_daily.md) | | | ✓ |
| [Risk Factor Z-Scores](schemas/09_risk_factor_zscores_daily.md) | | | ✓ |
| [AI Market Narrative](schemas/11_market_regime_quantgpt_daily.md) | | | ✓ |
| | | | |
| **History** | 36 months | Jan 2020 - Present | Jan 2020 - Present |
| **Refresh** | 30-day delay | After market close | After market close |
| **Total Views** | 7 | 7 | 13 |

Market Regime State has column-gating: Trial and Core receive core columns, Premium receives all columns.

---

## Index Coverage

All views cover the following indices:

| Index Name | Description |
|------------|-------------|
| Large Cap Core | Large cap broad market |
| Large Cap Growth | Large cap growth stocks |
| Large Cap Value | Large cap value stocks |
| Small Cap Core | Small cap broad market |
| Broad Market | Total market coverage |

---

## Get Access

[Request Access](https://altquant.ai/contact)
