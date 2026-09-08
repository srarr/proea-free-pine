# Reproduce one observation

The goal is to explain a visible state and record enough information for someone else to inspect it. No profitable outcome is required.

## Record the chart first

Record the indicator source hash, full exchange-qualified symbol, standard candlestick type, interval, timezone, visible/loaded date range, input changes, and whether the research lab is enabled. A saved chart image should leave the symbol, timeframe and script title readable.

Start with factory inputs and research off. Wait for the history and volume requirements. Find a confirmed trail flip and note its close time. Read the required/optional checks, then whether the paper slot and risk geometry allow acceptance. A rejected flip is a useful observation too.

For an accepted plan, record the saved entry, stop, checkpoint, target, quantity estimate and AT ENTRY reasons. On subsequent bars compare current context with that snapshot. Check whether the state now says MONITOR ONLY and whether the frozen levels remain intact. If stop and target occur in the same bar, inspect the ambiguity treatment described in the [accounting notes](formulas-and-limits.md).

## Small arithmetic example

This example is synthetic and does not claim to be a market backtest or a complete pressure-engine execution.

For `open=100`, `high=110`, `low=90`, `close=105`, close location is 0.5 and signed body efficiency is 0.25. With a valid volume window and relative volume 1.5, pressure is `(0.6*0.5 + 0.4*0.25)*1.5 = 0.6`. During a missing-volume recovery, weight 1 would yield 0.4 for context and new plans would remain blocked. An isolated candle cannot establish the warmed-up EMA, deviation, ATR, or ratchet state.

For a saved long paper plan with entry 104 and stop 96, initial price risk is 8. A later confirmed close at 112 has displacement `(112-104)/8 = +1R`. That does not move the stop, prove an executable price, or establish realized profit. A close at 100 gives `-0.5R` while the saved geometry remains the same.

## Useful comparisons

- Compare equality with a strict confirmed close through the prior active band; a wick alone should not be interpreted as confirmation.
- Compare required checks with informational checks; changing settings recalculates the study and is not the same historical experiment.
- Compare a current context direction with an older active plan's saved direction; they can differ.
- Treat a missing-volume recovery or incomplete research window as a valid limitation, not an invitation to hide the unfavorable case.

To report a discrepancy, follow [CONTRIBUTING.md](../CONTRIBUTING.md). Remove account identifiers, private chart names, API keys and other secrets from any attached material.
