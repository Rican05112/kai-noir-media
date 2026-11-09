# Trading rules

These are YOUR rules. Edit them by hand. The bot reads this file at the start of
every run and is not allowed to change it (see `.claude/settings.json`).

**Paper account only. This is a learning experiment, not a way to make money.**

## Watchlist

<!-- Replace with ~5 large, well-known stocks or ETFs you want to practice on.
     These are placeholders, not recommendations. -->
- SYMBOL_1
- SYMBOL_2
- SYMBOL_3
- SYMBOL_4
- SYMBOL_5

## Rules

1. **Position size:** max 10% of total account equity in any one position.
2. **Stop loss:** every buy has a stop loss attached when the order is placed.
3. **Order limit:** at most 3 orders per day (count today's entries in `trades-log.md`).
4. **No options, no shorting, no margin.** Only buy with cash you have; only sell what you own.
5. **Market hours:** only trade while the market is open (check `get_clock`).
6. **When in doubt, hold.** If any rule would be broken, or the data is unclear, the action is `hold`.
7. **Paper only:** if anything suggests this is not a paper account, stop and log it.
