# Binance Agent Skills

Agent skills for trading on Binance using the [binance](https://www.npmjs.com/package/binance) npm package. Works with Cursor, Claude Code, Codex, and other AI agents.

## Install

```bash
npx skills add YOUR_ORG/binance-agent-skills
```

Or copy the URL and ask your AI assistant:

> Please read https://raw.githubusercontent.com/YOUR_ORG/binance-agent-skills/main/SKILL.md, save it as a skill, and help me trade on Binance.

## Capabilities

| Module  | Covers                                           |
|---------|--------------------------------------------------|
| **Spot**    | Market/limit orders, cancel, balances             |
| **Futures** | USD-M & COIN-M positions, leverage, TP/SL        |
| **Account** | Balances, transfers, deposit addresses           |
| **Market**  | Prices, orderbook, klines, funding (no auth)     |

## Prerequisites

- Node.js
- `npm install binance`
- Binance API key (Read + Trade) — use env vars `BINANCE_API_KEY`, `BINANCE_API_SECRET`

## License

MIT
