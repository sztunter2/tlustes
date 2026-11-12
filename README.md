# TomStocks market data

Data for the TomStocks apps (Android and Windows), rebuilt every evening after the US close by the TomStocks
data pipeline. The apps download these files directly; nothing here is edited by hand.

| Path | Contents |
|---|---|
| `signals.csv` | The last five signals per ticker and interval: `area, ticker, interval, signal, candles_from_last, strength, partial, partial_frac` (`strength` = each listed candle's signal strength 1–5; `partial` = found on a candle that is still forming) |
| `data_<area>/<TICKER>_<interval>.csv` | Candles (`1d`, `1w`, `1M`): `Date, Open, High, Low, Close, Volume`, the bullish `signal` (= `bull_patterns`) and bearish `bear_patterns` (shooting star, hanging man, dark cloud cover, evening star, bearish engulfing, `RSI_OVERBOUGHT`), `strength` (1–5, 0 = no signal) and `strength_why` (the reasons), the same for the bearish signals (`bear_strength`, `bear_strength_why`), and for companies the per-share figures `eps_ttm, dps_ttm, sps, bps` |
| `signal_rules.json` | The rules behind the signals and their strength, and when the files were built |
| `fundamentals.csv` | One row per company: valuation, dividend, analysts, earnings dates and track record |
| `fundamentals/<area>/<TICKER>.json` | Everything known about a company: statements, earnings history, estimates, holders |

Areas: `sp500` (US), `europe`, `poland`, `currencies`, `indexes`, `crypto`.

## Signal strength (v2)

Each signal gets a **score**: one point for every confirmation that holds (for a bullish signal: a drop of 2 ATR in
5 bars or 3 ATR in 20, RSI(2) under 15 / 10 / 5, volume over 1.2× / 1.5× / 2× or under 1× its average, ADX ≥ 20 with
+DI above −DI, EPS growing year on year, a candle pattern together with RSI oversold, no red arrow in the last 3 bars,
S&P 500 member, the low within 1 ATR of EMA50 / EMA200, the low 20% of the 55-bar channel, trend score 3, RSI(14) ≤ 35,
the last report a beat). Bearish signals are scored by the mirror of these.

| Strength | Bullish | Bearish |
|---|---|---|
| 5 Strong | high score, RSI oversold or bullish engulfing, liquid, price ≥ 3, not after a 20-bar fall of 25%, above EMA200, last report not a miss | RSI overbought or bearish engulfing, liquid, not after a 20-bar rise of 25%, below EMA200, last report not a beat |
| 4 Good | high score, a candle pattern (or RSI oversold on weekly / monthly), liquid | the same for bearish patterns |
| 3 Medium | a high score that misses a gate of 4 and 5 | the same |
| 2 Weak | the rest | the rest |
| 1 Risky | no confirmation at all | no confirmation at all |

The score thresholds are in `signal_rules.json`. They are set so that S&P 500 daily gets about one 5, two 4 and five 3
a day. Strength describes the setup and is not a promise: plenty of strong signals still lose.

Prices and company figures come from Yahoo Finance. Provided as is, for information only; not investment advice.
