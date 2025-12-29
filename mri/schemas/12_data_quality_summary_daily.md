# Data Quality & Coverage Summary

Coverage, freshness, and integrity indicators for all MRI datasets. Use this to validate data completeness before analysis and to identify any gaps in pipeline coverage.

## What This Answers

Is the data complete and current?

## Data Structure

One row per trading day. Primary key is market_date.

Refresh is daily after US market close.

Coverage:
- Trial: Last 12 months with 30-day delay
- Core/Premium: Full history from January 2020

## Signal Characteristics

Quality metrics are deterministic. Coverage should be consistently high on normal trading days. Lower coverage on holidays or non-trading days is expected.

## Who Uses This

Data quality monitoring systems check this view before downstream processing. Pipeline health dashboards surface freshness metrics. Automated alerting systems trigger on quality score drops. Data validation workflows filter out incomplete days before analysis.

## Quality Score Interpretation

**90-100** — Excellent. Full data available, safe to use without reservation.

**75-90** — Good. Minor gaps that are acceptable for most use cases.

**50-75** — Fair. Some analysis may be limited. Worth investigating what's missing.

**Below 50** — Poor. Use with caution and investigate the gaps before proceeding.

## Columns

**market_date** (DATE) — Trading date for quality assessment.

**pipeline_freshness_hours** (INTEGER) — Hours since the latest data update. Lower is better. On a normal day this should be in single digits after market close.

**indices_coverage_pct** (DECIMAL) — Percentage of expected indices with data present. Should be 100% on normal trading days.

**breadth_coverage_pct** (DECIMAL) — Percentage of expected breadth data available. Should be 100% on normal trading days.

**macro_coverage_pct** (DECIMAL) — Percentage of macro fields populated. May lag on some economic releases since certain indicators publish with a delay.

**risk_appetite_coverage_pct** (DECIMAL) — Percentage of expected risk appetite pairs with data. Should be 100% on normal trading days.

**overall_quality_score** (DECIMAL) — Weighted composite data quality score. Range 0-100. This is the primary metric for data reliability assessment.

## Working With This Data

If the quality score drops below your threshold on a trading day, investigate before using data from that day in analysis. Common causes include delayed vendor feeds, holiday schedules, or pipeline issues.

Macro data may lag by a day for some indicators. This is expected behavior, not a data quality issue.

Use this view to filter out incomplete days when running historical analysis. Including partial data days can skew results.

Holiday and non-trading days will naturally have lower coverage. Don't alert on those.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    OVERALL_QUALITY_SCORE,
    INDICES_COVERAGE_PCT,
    BREADTH_COVERAGE_PCT,
    MACRO_COVERAGE_PCT
FROM ALTQUANT_MRI.CORE.DATA_QUALITY_SUMMARY
WHERE OVERALL_QUALITY_SCORE < 95
ORDER BY MARKET_DATE DESC
LIMIT 10;
```

## Related Views

All other views are affected by data quality. Check this view first when troubleshooting unexpected results.
