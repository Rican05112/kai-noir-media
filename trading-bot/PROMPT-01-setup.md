# Prompt 01 — Set up the folder and guardrails (run once, by hand)

> The files this prompt creates (`.claude/settings.json`, `trading-rules.md`,
> `trades-log.md`) are already in this folder. You can still paste this into
> Claude Code here to check the connection and get the plain-English walkthrough;
> Claude will merge rather than overwrite.

```
I'm setting up a paper-trading bot with Alpaca's MCP server. Paper money only, for learning.

1. Check the alpaca MCP server is connected and call get_account_info. Tell me the buying power, and remind me this should be a paper account.
2. Add these permission rules to .claude/settings.json in this folder (merge with anything already there):
   allow: mcp__alpaca__get_*
   ask: mcp__alpaca__place_stock_order, mcp__alpaca__place_crypto_order, mcp__alpaca__cancel_all_orders, mcp__alpaca__close_position
   deny: mcp__alpaca__place_option_order, mcp__alpaca__close_all_positions, mcp__alpaca__update_account_config
3. Create trading-rules.md with these rules for me to edit:
   - Watchlist: [e.g. 5 large, well-known stocks or ETFs]
   - Max 10% of the account in any one position
   - Every buy has a stop loss
   - At most 3 orders a day
   - No options, no shorting, no margin
   - Only trade while the market is open
4. Create an empty trades-log.md.
5. Explain each rule and permission back to me in plain English.

Never ask me for API keys and never change ALPACA_PAPER_TRADE.
```
