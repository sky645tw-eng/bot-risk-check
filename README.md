# bot-risk-check

Free skill for Claude Code / Cursor / Codex.
Audits trading-bot source for missing stop loss, position cap, kill switch, and hardcoded keys.

Not a signal. Not copy-trading. No profit claim.
Verdicts: `BLOCK-LIVE` | `PAPER-ONLY` | `FIX-THEN-PAPER`.

## Install (Claude Code)

```bash
git clone https://github.com/sky645tw-eng/bot-risk-check.git
mkdir -p ~/.claude/skills
cp -r bot-risk-check ~/.claude/skills/bot-risk-check
```

Windows: copy the folder to `%USERPROFILE%\.claude\skills\bot-risk-check`

Cursor: copy to `.cursor/skills/bot-risk-check/` in the project.

Restart the agent session. Paste bot code and say: `use bot-risk-check`.

## What BLOCK-LIVE means

Missing hard SL, size cap, circuit breaker, or secrets in source → do not go live.

## License

MIT
