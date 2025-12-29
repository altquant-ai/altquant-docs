# Market Index OHLC (Daily)

Daily OHLCV price data for market indices. Provides the price backbone for overlaying regime signals and calculating returns/drawdowns.

## What This Answers

What was the index price on this date?

## Data Structure

One row per trading day per index. Primary key is market_date + index_name.

Refresh is daily after US market close.

Coverage:
- Trial: Last 12 months with 30-day delay
- Core/Premium: Full history from January 2020

## Signal Characteristics

Price data is objective and deterministic. All prices are split and dividend adjusted for accurate historical comparison.

## Who Uses This

Price overlays on regime charts. Return and drawdown calculations. Backtesting signal effectiveness. Technical analysis foundations.

## Columns

**market_date** (DATE) — Trading date.

**index_name** (VARCHAR) — Market index identifier. Valid values: Large Cap Core, Large Cap Growth, Small Cap Core, Large Cap Value, Broad Market.

**index_etf** (VARCHAR) — Primary ETF ticker representing the index. Valid values: SPY, QQQ, IWM, DIA, IWB.

**adj_open** (DECIMAL) — Split and dividend adjusted opening price.

**adj_high** (DECIMAL) — Split and dividend adjusted high price.

**adj_low** (DECIMAL) — Split and dividend adjusted low price.

**adj_close** (DECIMAL) — Split and dividend adjusted closing price.

**adj_volume** (BIGINT) — Adjusted trading volume.

## Working With This Data

Use adj_close for most analytics including returns, overlays, and drawdowns.

Index names match those used in other MRI views, making joins straightforward.

Volume is raw share count, not split-adjusted.

## Example Query (Snowflake)

```sql
SELECT
    MARKET_DATE,
    INDEX_NAME,
    INDEX_ETF,
    ADJ_CLOSE,
    ADJ_VOLUME
FROM ALTQUANT_MRI.CORE.MARKET_INDEX_OHLC_DAILY
WHERE INDEX_NAME = 'Large Cap Core'
ORDER BY MARKET_DATE DESC
LIMIT 30;
```

## Related Views

- Market Regime State — Regime classification for overlay
- Market Stress Flags — Stress indicators
- Market Participation — Breadth metrics
