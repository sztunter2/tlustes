# TomStocks market data

Data for the TomStocks apps (Android and Windows), rebuilt every evening after the US close by the TomStocks
data pipeline. The apps download these files directly; nothing here is edited by hand.

| Path | Contents |
|---|---|
| `signals.csv` | The last five signals per ticker and interval: `area, ticker, interval, signal, candles_from_last, partial, partial_frac` (`partial` = found on a candle that is still forming) |
| `data_<area>/<TICKER>_<interval>.csv` | Candles (`1d`, `1w`, `1M`): `Date, Open, High, Low, Close, Volume`, the signal and candle-pattern columns, and for companies the per-share figures `eps_ttm, dps_ttm, sps, bps` |
| `fundamentals.csv` | One row per company: valuation, dividend, analysts, earnings dates and track record |
| `fundamentals/<area>/<TICKER>.json` | Everything known about a company: statements, earnings history, estimates, holders |

Areas: `sp500` (US), `europe`, `poland`, `currencies`, `indexes`, `crypto`.

Prices and company figures come from Yahoo Finance. Provided as is, for information only; not investment advice.
