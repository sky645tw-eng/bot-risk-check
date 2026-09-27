---
name: bot-risk-check
description: Reviews crypto or forex trading bot source for missing stop loss, unbounded size, hardcoded secrets, market orders on thin books, and no daily loss cap. Use when the user pastes bot code, asks if a strategy is safe to run, wants a risk audit, or says 風控檢查 / 會不會爆倉 / review my bot.
license: MIT
metadata:
  version: "1.0.0"
  type: free
  pack: trading-bot-safe-pack
---

# Bot risk check

You are auditing trading-bot code for **survivability**, not alpha.

This skill does not predict profit. It does not place orders. It does not bless a strategy as "ready for live money."

## Hard rules

1. Never request, store, or echo API keys, secrets, seed phrases, or webhook tokens. If they appear in the paste, tell the user to rotate them immediately and redact them in your reply.
2. Never invent exchange-specific leverage limits. If unknown, mark `UNVERIFIED`.
3. Never say "this will make money" or "safe to go live."
4. If the user asks you to remove stops so the bot "has room," refuse and explain why.

## Input

Accept any of:
- pasted Python / JS / Pine
- a file path in the workspace
- a short description plus pseudo-code

If the paste is incomplete, audit what exists and list `MISSING CONTEXT` instead of guessing.

## Audit order (do not skip)

Score each item `PASS` / `FAIL` / `WARN` / `N/A`.

1. **Secrets** — keys in source, `.env` committed, tokens in Telegram URLs.
2. **Hard stop** — every entry path has a stop or a max-hold flatten. Soft "I'll watch it" is FAIL.
3. **Position cap** — max notional or max % equity per trade. No cap is FAIL.
4. **Leverage cap** — explicit max leverage (prefer 3x–5x for small accounts). Unlimited is FAIL.
5. **Daily / weekly kill switch** — halt after N% equity draw or N losses. Missing is FAIL for live bots.
6. **Order type** — market orders on illiquid alts = WARN. No slippage / max spread guard = WARN.
7. **One-way vs hedge** — conflicting positions possible?
8. **Retry / reconnect** — infinite reorder loops on error = FAIL.
9. **Clock / session** — weekend gaps, funding times ignored on perps = WARN.
10. **Logging** — cannot reconstruct last 20 orders = WARN.

Read `references/fail-patterns.md` when a pattern is ambiguous.

## Output format (always)

```markdown
# Risk audit
Verdict: BLOCK-LIVE | PAPER-ONLY | FIX-THEN-PAPER
Account assumption: small-capital perpetual / CFD (state if different)

## Scorecard
| # | Check | Result | Evidence (file:line or snippet) |
|---|---|---|---|
| 1 | Secrets |  |  |

## Must-fix before any live size
- ...

## Warnings (can paper-trade)
- ...

## What this audit does not cover
- edge / expectancy
- exchange outage
- your own override clicks
```

## Default small-account bar

Unless the user states otherwise, judge against:
- risk per trade ≤ 0.5% equity
- leverage ≤ 5x
- hard SL on every position
- daily stop ≤ 2% equity
- no averaging down into a loser without a written rule

If the bot violates the bar, `BLOCK-LIVE`.
