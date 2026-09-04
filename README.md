# Daily must-go jackpot — levels designer

A single-page tool for designing the per-interval odds table behind a daily must-go jackpot: a
jackpot that is guaranteed to drop before the day closes. Open `index.html` and everything runs in
the browser. No build step, no server, no dependencies.

## What it does

You give it a design point — the daily volume you expect and the share of jackpots you want landing
in the closing hours — and it solves for the levels table that produces exactly that, then stress-
tests the result against real day-to-day volatility.

Each interval carries an odds denominator, "1 in N per unit of stake". Every unit staked is an
independent trial, so the hit distribution is a closed form rather than a simulation, and the whole
page recalculates instantly as you type. The final interval is always 1-in-1, which is what makes
the jackpot must-go.

## Inputs

| Input | Meaning |
| --- | --- |
| Design stake | Daily volume the table is tuned for. 1 unit = 1 dollar of stake. |
| Target share | Share of jackpots that should land inside the closing window, at the design stake. |
| Final window | Length of that closing window, in hours. |
| Intervals per day | How finely the day is sliced. 144 gives 10-minute intervals. |
| Forced-dump share | Probability the closing 1-in-1 interval is the one that has to release it. |
| Early ramp | How much the hit rate rises across the morning. Higher is quieter early. |
| Tolerance band | The share range you are willing to accept on any given day. |

## What it shows

- **Hit-time distribution** — the probability the jackpot is won in each hour, at any of eight preset
  daily volumes, overlaid against the design point and both edges of the tolerance band. Volume is
  the only thing that moves this curve: more stake burns through the odds faster and pulls the
  jackpot earlier in the day.
- **Stake sensitivity** — final-window share and forced-dump rate across the full volume range, with
  the tolerance band shaded, so you can read off the volume range a single table survives.
- **Stress test** — replays real observed daily volatility against your table and counts how many
  days land inside the band, with the misses broken out into too-early and too-late.
- **The table itself** — all intervals, downloadable as CSV.

## Volatility profiles

The bundled profiles are the real day-to-day volatility of six live titles, stored **only as ratios
to each title's own median**, with the first 21 days after launch excluded so they describe settled
behaviour. There are no absolute figures in this repository. You set the median you are planning
for and the profile is rescaled to it, so the same shape works at any volume.

You can also paste your own daily stake figures instead, one per line.

## Sharing

Every input is serialised into the URL fragment, so "Copy share link" gives you a link that opens
the exact configuration you are looking at. Nothing is stored or transmitted.

## A note on the design point

The band a single table can hold is roughly 2.6x wide in volume. Real games swing more than that,
and a launch typically runs several times settled volume before decaying, so a table tuned for
settled conditions will resolve early during launch. The stress test makes that trade-off explicit
rather than hiding it — the honest answer is usually to pick the design point that maximises days
inside the band, which is not always the median you expect.
