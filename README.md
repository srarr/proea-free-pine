# Free Pine tools from ProEA Lab

This repository starts with **Drift Desk 1.3.0**, an open-source Pine Script v6 indicator for reading candle pressure, a confirmed trend trail, and fixed paper-plan references on a TradingView chart.

The indicator is complete and works without a website account. It is an inspectable chart tool, not a validated profitable trading system. Source and documentation are licensed under [MIT](LICENSE).

## Install Drift Desk

1. Open a standard time-based candlestick chart in TradingView on a feed with volume.
2. Open Pine Editor and create a new indicator.
3. Copy the complete [drift-desk.pine](drift-desk.pine) file into the editor, save it, then select **Add to chart**.
4. Begin with the shipped inputs. Load enough history for calculations and any required higher-timeframe reference to become available.
5. Read the current state and the reason shown in the desk. A trail flip is only a candidate; it may fail the data, risk, optional checks, or paper-occupancy rules.

Saving a personal script is different from publishing a community script. No public TradingView publication is claimed by this repository.

## What is included

| File | Purpose |
| --- | --- |
| [drift-desk.pine](drift-desk.pine) | Full v1.3.0 indicator, unchanged from the documented upstream download |
| [LICENSE](LICENSE) | Complete MIT permission and copyright notice |
| [Formulas and limits](docs/formulas-and-limits.md) | Pressure/trail rules, data handling, paper accounting, and research boundaries |
| [Reproduce one observation](docs/reproduce-an-observation.md) | A practical chart-reading checklist and an arithmetic example |
| [Provenance](PROVENANCE.md) | Version, exact source hash, origin, and the limits of this release review |
| [Contribution guide](CONTRIBUTING.md) | What to include in a useful bug report or proposed change |

The separate strategy companion is not included in this first repository release. It uses a different broker-emulator ledger and needs its own review; indicator paper outcomes should not be treated as broker results.

## First chart workflow

Read **RUN** to confirm the inputs. Follow one confirmed trail flip and inspect which checks were required. If a plan is accepted, **NEW ON LAST CLOSE** describes the acceptance bar; an older plan becomes **MONITOR ONLY**. Its saved levels and **AT ENTRY** reasons remain fixed while current context changes.

The 1R line records a checkpoint. It does not move the stop or take a partial exit. The latest closed-price displacement in original R describes the open paper plan; it is not realized profit or the reward/risk of entering late.

Use Compact/Small or hide panels if the full desk covers your chart. The panel describes the latest state; moving the cursor to an older candle does not turn it into a historical dashboard.

## Documentation

- [Visual Drift Desk manual](https://proea.app/en/free/drift-desk)
- [Pine installation walkthrough](https://proea.app/en/blog/how-to-add-pine-script-to-tradingview)
- [Browse the free tools](https://proea.app/free?utm_source=github&utm_medium=referral&utm_campaign=free_pine_pilot)

The code and explanations here stand on their own; those links provide additional visual walkthroughs and related tools. Source usage does not require registration, a purchase, or a backlink.

## Limits to read before interpreting the chart

Pressure is derived from candle OHLC and volume, not bid/ask order flow. Context agreement is not a probability. Historical and paper observations do not establish future performance. Indicator paper results exclude costs; the optional research lab applies a separate simplified cost model. Quantity estimates use symbol metadata, not account-specific broker rules or currency conversion.

Confirmed decisions add delay. A forming candle is provisional, and changes to the feed, history, or inputs can recalculate the study. Live alert delivery, broker routing, and mobile TradingView layouts were not established by this repository's packaging review. See the detailed [limits](docs/formulas-and-limits.md).
