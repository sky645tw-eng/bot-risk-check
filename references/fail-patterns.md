# Fail patterns

## Instant FAIL

- `apiKey = "xxxxx"` or secrets in repo
- `leverage = 20` or higher with no cap function
- `create_order(..., type="MARKET")` with no max size
- no `stopLoss` / `sl` / protective order on entry path
- `while True: place_order()` with no circuit breaker
- martingale / double-down with no max adds

## Usually WARN

- only trailing stop, no initial stop
- TP exists but SL is a comment
- risk % is a variable defaulting to 2%+
- Telegram alerts contain full order dumps with keys in query string
- backtest uses look-ahead (`shift(-1)` on the signal)

## Language for the user

Say: "This is a process check. Passing it means fewer ways to blow up, not that the strategy has edge."
