# TomStocks market data

Data for the TomStocks apps (Android and Windows), rebuilt every evening after the US close by the TomStocks
data pipeline. The apps download these files directly; nothing here is edited by hand.

| Path | Contents |
|---|---|
| `signals.csv` | The last five signals per ticker and interval: `area, ticker, interval, signal, candles_from_last, strength, partial, partial_frac` (`strength` = each listed candle's signal strength 1–5; `partial` = found on a candle that is still forming) |
| `data_<area>/<TICKER>_<interval>.csv` | Candles (`1d`, `1w`, `1M`): `Date, Open, High, Low, Close, Volume`, the bullish `signal` (= `bull_patterns`) and bearish `bear_patterns` (shooting star, hanging man, dark cloud cover, evening star, bearish engulfing, `RSI_OVERBOUGHT`), `strength` (1–5, 0 = no signal) and `strength_why` (the reasons), and for companies the per-share figures `eps_ttm, dps_ttm, sps, bps` |
| `signal_rules.json` | The rules behind the signals and their strength, and when the files were built |
| `fundamentals.csv` | One row per company: valuation, dividend, analysts, earnings dates and track record |
| `fundamentals/<area>/<TICKER>.json` | Everything known about a company: statements, earnings history, estimates, holders |

Areas: `sp500` (US), `europe`, `poland`, `currencies`, `indexes`, `crypto`.

## Signal strength

| Strength | Meaning |
|---|---|
| 5 Strong | The stock is a momentum leader (12-1 month return in the top 20% of its market), no earnings report for over 30 days, RSI oversold |
| 4 Good | Leader, no report for over 30 days, candle pattern |
| 3 Medium | Leader with a report in 8–30 days (or no earnings dates known) |
| 2 Weak | Not a leader: no proven edge |
| 1 Risky | An earnings report within 7 days |

Weekly and monthly signals are at most 4, illiquid stocks at most 2, and a signal on a candle that is still
forming is one lower. Currencies, indexes and crypto have no leader ranking (2 or 1). Strength is a historical
average, not a promise: plenty of strong signals still lose.

Prices and company figures come from Yahoo Finance. Provided as is, for information only; not investment advice.
