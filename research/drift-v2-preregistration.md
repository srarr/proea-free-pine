# Drift research v2: pre-registration fingerprints

Posted 2026-09-24 by ProEA Lab, **before the final test of v2 runs**.

The next test asks whether a long-only trend rule can beat **simply holding the same coins with the
same capital**, after costs, on 12 coins that played no part in choosing it. We wrote the test's
rules and its pass rule and committed them in our lab before any of the test's data was downloaded.
We post their SHA-256 fingerprints here so that, when we publish the files with the result, anyone
can check that they did not change.

| File | SHA-256 | Committed in our lab (UTC+7) |
|---|---|---|
| `PROTOCOL-v2.md` (the rules, incl. amendment v2-A1) | `fc227467488eecea648680caedb2bd86ed9c746481a248d528bb16689baaa802` | 2026-09-24 11:06 |
| `DEV-GRID-v2.md` (the 10 configurations tested) | `70dc78a16cba29a82278c87ac7112ccfb2bf4fb701a2375a9494725bea2e5940` | 2026-09-24 10:01 |
| `gate-v2.mjs` (the pass rule) | `7409d3678c0dc9e66a71f4a9d993d128cc141f037773d957d8aed8ccff98570a` | 2026-09-24 10:01 |

## The pass rule, in plain words

On the 12 final-test coins, all of these must hold:

1. Net result after costs is above zero.
2. Profit per unit of drawdown is at least 1.2 times that of holding the same coins with the same
   average capital (constant exposure).
3. Profit factor is at least 1.2.
4. At least 7 of the 12 coins are profitable.
5. The result stays above zero when costs are doubled.
6. The timing part of the result stays above zero at the lower end of a 95% bootstrap interval
   (compared with random entries held for the same time).

If any condition fails, we publish the result as a failed test, as we did for the previous one.

## Status when posted

- The final test on the 12 coins has **not** run.
- Development and selection on other coins are in progress.
- No performance claim is made here. All results will be hypothetical backtests, not a live record.

## How to check

When the files are published, download them and compare their fingerprints:
`sha256sum PROTOCOL-v2.md DEV-GRID-v2.md gate-v2.mjs` (Windows: `Get-FileHash -Algorithm SHA256 <file>`).
They must equal the table above. This file's own commit time on GitHub is the public timestamp.

## Amendment v2-A2 (posted before the final test runs)

The rules file was amended once after the fingerprints above were posted. The **pass rule
(`gate-v2.mjs`) and the tested configurations (`DEV-GRID-v2.md`) did not change.**

| File | New SHA-256 | Committed in our lab (UTC+7) |
|---|---|---|
| `PROTOCOL-v2.md` (rules incl. amendments v2-A1 and v2-A2) | `5ce2e6c473969d54eadb8eaddf832563b0c846e99bc18f8257c463614f1b264b` | 2026-09-24 17:46 |

What v2-A2 does, made after development and selection on other coins and **before any result on the
12 final-test coins**:

- keeps ICX in the final coin set, as the written selection rule requires, and corrects the rules'
  statement that every final-test coin was still trading (Binance delisted ICX on 2026-09-03);
- fixes how an open position and the "holding" benchmark end when a coin's data stops early;
- fixes the final test's scope (one configuration, run once, with its comparisons and cost scenarios);
- discloses that some selection readings were fixed after development results were seen.

Status when this was posted: the final test on the 12 coins has **not** run.

## Correction (posted 2026-09-24, after the final test ran)

Two sentences above say more than our records support. This section corrects their wording only:
the fingerprints, the pass rule (`gate-v2.mjs`) and amendment v2-A2 are unchanged.

1. **"...committed them in our lab before any of the test's data was downloaded."** This holds only
   for the 12 final-test coins. By our records, we committed the rules and the pass rule at 10:01
   (UTC+7) and amendment v2-A1 at 11:06, and downloaded the 12 final-test coins' data later that day
   (its data record was committed at 14:16). The coins used for development and selection had been
   downloaded earlier, for our first Drift test. Read the sentence as: *we committed the rules and the
   pass rule in our lab before any v2 result, and before the 12 final-test coins' data was
   downloaded.*

2. **"...simply holding the same coins with the same capital."** The benchmark in the pass rule is
   not buy-and-hold. It keeps a constant USDT value in each coin, equal to the strategy's own average
   capital in that coin, and trades back to that value at every 4-hour close. For the pass rule it
   pays costs on its first entry and last exit only, not on the rebalancing trades. Rebalancing like
   this gains or loses on its own, so it is a different baseline from buying once and holding. Read
   the question as: *can a long-only trend rule, after costs, earn more per unit of drawdown than
   that constant exposure?*
