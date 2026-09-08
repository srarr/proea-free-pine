# Drift Desk 1.3.0: formulas and limits

These notes describe the shipped indicator. They explain the implementation; they do not establish a trading edge.

## Candle pressure and the trail

Let a candle have open `o`, high `h`, low `l`, close `c`, and volume `v`. For a positive range `h-l`, close location is `(2*c-h-l)/(h-l)` and signed body efficiency is `(c-o)/(h-l)`, each clamped to `[-1,1]`. A zero range returns zero for both.

Relative volume is `v / SMA(v, volumeLen)` only when the complete window contains positive volume observations. The default window is 20 bars and the volume multiplier is capped at 3. Missing or zero volume restarts that recovery requirement. Until it recovers, neutral multiplier 1 preserves price-only context while blocking new plans; existing plans continue through their normal outcome rules.

`pressure = (0.6*closeLocation + 0.4*bodyEfficiency) * volumeWeight`

The fast/slow pressure EMAs default to 8/21. Their difference divided by its 50-bar standard deviation is clamped to `[-3,3]`; zero deviation yields a zero drift value. The projected center is `hlc3 + drift*ATR*projection`, with default ATR 14 and projection 0.5. Raw bands sit two ATRs from that center by default.

The lower band ratchets upward when the preceding close is at or above its previous lower band; the upper band mirrors this logic. An upward state flips only when the confirmed close is strictly below the previous lower band. A downward state flips only when the confirmed close is strictly above the previous upper band. A wick touch or equality does not cause a flip. Initializing a direction does not emit a flip.

## Candidate versus accepted plan

A confirmed flip must also pass common history readiness, positive-volume recovery, valid ATR/stop/target/quantity rules, and any enabled optional checks. The five optional checks are chart EMA alignment, price-path efficiency, eligible-frame agreement, a New York entry session, and a completed higher-timeframe EMA reference. All five are off by default.

Turning checks off does not remove the data, geometry, or paper-occupancy requirements. A rejected flip is not queued for later. The indicator accepts at most one active paper plan and does not open a new plan on a bar that resolves the old one.

Malformed confirmed chart OHLC produces a runtime error before the chart-bar state is used. All four values must exist and open/close must fall inside the high-low range. Coherent flat, zero and negative-price bars can still be valid OHLC, although ATR/risk requirements can block a plan. This guard does not independently validate every raw higher-timeframe feed component.

## Higher-timeframe context

The eight fixed rows are 1, 5, 15, 30, 60, and 240 minutes, daily and weekly. Their proxy uses EMA20/EMA50 alignment and EMA50's three-bar slope normalized by ATR14. This is separate from the pressure engine.

Exact chart rows use confirmed local values. Eligible source rows use the preceding completed source candle. Lower-duration rows are discarded; unavailable rows are excluded. Available neutral rows stay in the denominator of agreement. Calendar identity matters: equal durations do not automatically make a 1440-minute chart a 1D chart or a 7D chart a 1W chart.

The optional HTF EMA gate defaults to a 60-minute EMA21 reference. When enabled it must be strictly above the chart duration. The confirmed chart close must be strictly above that completed reference for a long candidate, or below it for a short candidate. Missing reference history blocks the candidate. Source confirmation adds delay; displayed age is not a guarantee of source freshness.

## Fixed paper geometry and accounting

For a long plan the stop begins at the lowest low of the prior 10 completed bars minus 0.25 ATR; shorts mirror this with the highest high plus the buffer. The stop is rounded outward to a symbol tick. Entry is the accepted confirmed close. The observational checkpoint is 1R and the final target defaults to 2R; outward target rounding can make the actual multiple slightly larger.

The default cash-risk input is 100 in symbol currency. Estimated units are rounded down to the selected quantity increment using symbol point value. Auto uses positive minimum-contract metadata, otherwise 1; Manual uses the supplied increment. This does not validate an actual brokerage account, capital availability, or exchange-rate conversion.

Paper outcomes start on bars after entry. An opening gap through the stop resolves at the open. An opening price beyond the target resolves at the target. Otherwise, a bar touching both stop and target is recorded stop-first and marked ambiguous. A surviving plan times out at the close after the configured number of bars, default 120. The 1R checkpoint never changes stop, target or quantity. Levels remain frozen even when the trail changes direction.

The indicator paper record is gross, before trading costs. Its OHLC ordering assumptions cannot recover the actual intrabar path or establish a fill. An open plan's signed original-R displacement is `direction*(latestClosedPrice-savedEntry)/savedInitialRisk`; it is not a closed-trade result.

## Optional research lab

Research is off by default. It compares 12 fixed cells: ATR lengths 10/14/21 crossed with band multipliers 1.5/2/2.5/3. The default training start is 2026-01-01 UTC, the split 2026-06-01, and validation end 2026-09-01. Change dates deliberately before examining later data.

Selection reads training books only, requiring covered history and the configured minimum completed training plans (20 by default). It ranks mean net R, then lower closed-trade drawdown, with stable ties. The chosen cell is frozen for comparison with the manual baseline. No cell is adopted into the user's operating inputs. Incomplete history or no eligible leader is a legitimate outcome.

The lab deducts a simplified price-based cost drag: default 0.04% on each side plus one adverse tick on each side. That deduction does not move the modeled path or simulate actual fills. Drawdown is calculated from closed-plan R, not intratrade equity. Repeatedly looking at or tuning to the validation window consumes its independence. The minimum count is a filter, not statistical significance or proof of future performance.

## Alerts and presentation

Named alert conditions cover long/short accepted paper plans, a raw trail flip, 1R observation and paper resolution. The optional structured `alert()` event represents an accepted paper plan and is emitted once per confirmed close. It is not a broker order or a confirmed fill.

Alert snapshots retain saved source/settings. Recreate them after changes and record configuration separately: event IDs do not fully identify input settings. This package review did not create alerts or test live delivery.

Pressure words such as “leaning buy” and “strong sell” label the pressure score, not advice to trade or a success probability. PLAN labels can be horizontally displaced for legibility; their x-position is not entry time. Rendering can be crowded on small screens, session gaps, manual/log scales, or particular chart zooms. Adjust density, size and position; phone/native-mobile behavior is not certified here.
