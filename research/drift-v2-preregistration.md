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
