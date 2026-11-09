# Alpaca paper-trading bot (Claude Code)

Claude, on a schedule, reads market data and your **paper** account, decides
against rules you wrote, places orders on the paper account, and logs why.

> **Paper money only. For learning, not for making money.** Most active traders
> lose money, and paper results look better than real ones (no slippage, no
> market impact, no dividends, IEX-only data). Nothing here is financial advice.

## What's in this folder

| File | Purpose |
|---|---|
| `.claude/settings.json` | Permission guardrails (see below). |
| `trading-rules.md` | **Your** rules + watchlist. Edit the placeholder symbols before running. |
| `trades-log.md` | The bot's memory: one line per decision and its reason. |
| `PROMPT-01-setup.md` | One-time setup/check prompt. |
| `PROMPT-02-bot.md` | The prompt each (scheduled) run uses. |

Each scheduled run starts fresh, so the bot's only memory is `trading-rules.md`
and `trades-log.md`.

## Guardrails (`.claude/settings.json`)

Deny always wins over ask and allow.

- **allow** — `mcp__alpaca__get_*` (read-only account, positions, clock, bars, news),
  reading both markdown files, and editing `trades-log.md` so runs can log.
- **ask** — `place_stock_order`, `place_crypto_order`, `cancel_all_orders`,
  `close_position`. Every order waits for your yes.
- **deny** — `place_option_order`, `close_all_positions`, `update_account_config`.
  Also denied: editing `trading-rules.md` or anything in `.claude/` (so the bot
  can't loosen its own rules), and running `claude mcp …` (so it can't
  reconfigure the Alpaca server, e.g. flip `ALPACA_PAPER_TRADE`).

Tool names come from the guide for `alpaca-mcp-server` v2. After connecting, run
`/mcp` and check the alpaca tool list matches; adjust the names here if not.

Optional extra lock: add `--env ALPACA_TOOLSETS=account,trading,assets,stock-data,news`
to the `claude mcp add` command below so options/account-settings tools never load.

## Setup

1. **Paper account.** Sign up free at alpaca.markets, switch to the *Paper*
   account (upper left), open *API* and generate paper keys. The secret is shown
   once — store it privately. **Never commit keys to this repo or paste them into chat.**
2. **Install** Claude Code (Pro plan or higher) and
   [uv](https://docs.astral.sh/uv/getting-started/installation/).
3. **Add the server** — type your own paper keys into this command in your terminal:
   ```sh
   claude mcp add --scope user \
     --env ALPACA_API_KEY=YOUR_PAPER_KEY \
     --env ALPACA_SECRET_KEY=YOUR_PAPER_SECRET \
     --env ALPACA_PAPER_TRADE=true \
     --transport stdio alpaca -- uvx alpaca-mcp-server
   ```
4. `cd trading-bot && claude`, run `/mcp` to confirm `alpaca` is connected, then
   ask *"What's my paper account balance?"*
5. Edit the watchlist in `trading-rules.md`. Optionally paste Prompt 01 for a
   connection check and plain-English walkthrough.
6. Run Prompt 02 by hand. Approve or reject each order. Read `trades-log.md`.
7. **Schedule** in Claude Desktop (v1.1.5368+): *Code* tab → *Scheduled* →
   *New task* → *Only on this computer* → pick this `trading-bot` folder →
   schedule (e.g. weekdays hourly during market hours) → paste Prompt 02.
   Keep order tools on **ask**. It only runs while the app is open and the
   computer is awake.
8. **Stay on paper.** Never set `ALPACA_PAPER_TRADE=false`, never use live keys.

## Links

- Alpaca MCP server: https://github.com/alpacahq/alpaca-mcp-server
- Alpaca paper trading: https://docs.alpaca.markets/docs/paper-trading
- Claude Code MCP: https://code.claude.com/docs/en/mcp
- Claude Code permissions: https://code.claude.com/docs/en/permissions
- CFTC on AI trading bots: https://www.cftc.gov/LearnAndProtect/AdvisoriesAndArticles/AITradingBots.html
