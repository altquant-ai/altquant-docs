# MRI View Catalog

Complete index of all MRI data views.

---

## Core Views

| View | Grain | Tier | Description |
|------|-------|------|-------------|
| [market_regime_state_daily](schemas/market_regime_state_daily.md) | date x index | All | Primary regime classification |
| [market_participation_daily](schemas/market_participation_daily.md) | date x index | All | Breadth and thrust signals |
| [risk_appetite_summary_daily](schemas/risk_appetite_summary_daily.md) | date | All | Risk-on/risk-off composite |
| [market_stress_flags_daily](schemas/market_stress_flags_daily.md) | date x index | All | Binary stress indicators |
| [macro_context_daily](schemas/macro_context_daily.md) | date | Core+ | Rates and liquidity backdrop |
| [data_quality_summary_daily](schemas/data_quality_summary_daily.md) | date | All | Data coverage metrics |

---

## Premium Views

| View | Grain | Tier | Description |
|------|-------|------|-------------|
| [risk_appetite_signals_daily](schemas/risk_appetite_signals_daily.md) | date x factor | Core+ | Individual factor readings |
| [market_breadth_structure_daily](schemas/market_breadth_structure_daily.md) | date x index | Core+ | Phase detection |
| [trend_convergence_daily](schemas/trend_convergence_daily.md) | date x index | Core+ | Divergence analysis |
| [risk_factor_zscores_daily](schemas/risk_factor_zscores_daily.md) | date x index | Core+ | Standardized risk factors |
| [risk_driver_contributions_daily](schemas/risk_driver_contributions_daily.md) | date x index x factor | Premium | Attribution rankings |
| [market_regime_quantgpt_daily](schemas/market_regime_quantgpt_daily.md) | date x index | Premium | AI narrative |

---

## View Categories

### Regime & State
- `market_regime_state_daily` - What regime are we in?
- `market_regime_quantgpt_daily` - What does it mean?

### Risk Appetite
- `risk_appetite_summary_daily` - Overall risk appetite
- `risk_appetite_signals_daily` - Factor-level detail

### Breadth & Participation
- `market_participation_daily` - Breadth metrics
- `market_breadth_structure_daily` - Phase analysis
- `trend_convergence_daily` - Divergence detection

### Risk Factors
- `risk_factor_zscores_daily` - Factor readings
- `risk_driver_contributions_daily` - Attribution

### Stress & Alerts
- `market_stress_flags_daily` - Binary triggers

### Context
- `macro_context_daily` - Macro backdrop
- `data_quality_summary_daily` - Data health

---

## Schema

All views are in the `marketintel` schema:

```sql
SELECT * FROM marketintel.<view_name>
```

---

## Get Access

[Request Access](https://altquant.ai/contact)
