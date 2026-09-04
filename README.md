# Daily must-go jackpot — levels designer

A single-page tool for designing the per-interval odds table behind a daily must-go jackpot: a
jackpot that is guaranteed to drop before the day closes. Open `index.html` and everything runs in
the browser. No build step, no server, no dependencies.

## What it does

The page opens on a shipped table of 144 ten-minute intervals, tuned for $125,000 a day with 80% of
jackpots landing in the closing four hours. From there you can work two ways: type odds straight
into the table to hand-tune it interval by interval, or change the design parameters and press
**Rebuild table from these** to solve for a fresh table.

Rebuilding is always an explicit click, so editing a parameter never silently discards work you
typed into the table. A pill next to the button tells you whether you are looking at the shipped
table or a custom one.

Each interval carries an odds denominator, "1 in N per unit of stake". Every unit staked is an
independent trial, so the hit distribution is a closed form rather than a simulation, and the whole
page recalculates instantly as you type. The final interval is 1-in-1, which is what makes the
jackpot must-go; the page warns you if an edit breaks that.

## Inputs

| Input | Meaning |
| --- | --- |
| Design stake | Daily volume the table is tuned for. 1 unit = 1 dollar of stake. |
| Target share | Share of jackpots that should land inside the closing window, at the design stake. |
| Final window | Length of that closing window, in hours. |
| Tolerance band | The share range you are willing to accept on any given day. |

The design stake and target share only feed **Rebuild**. The final window and tolerance band are
read-outs: they judge whatever table is currently loaded, hand-edited or not.

## What it shows

- **Hit-time distribution** — the probability the jackpot is won in each hour, at any of nine preset
  daily volumes from $50K to $1M, overlaid against the design point and both edges of the tolerance
  band. Volume is the only thing that moves this curve: more stake burns through the odds faster and
  pulls the jackpot earlier in the day. Hover a preset to read its share before switching.
- **The table itself** — every interval with its odds, per-interval hit chance and cumulative
  probability. Editable, downloadable as CSV, and resettable to the shipped table with one click.

## Sharing

Every input is serialised into the URL fragment, and so is a hand-edited table, so "Copy share link"
gives you a link that opens the exact configuration and odds you are looking at. Nothing is stored
or transmitted, and the page contains no game names, no observed volumes and no absolute figures
beyond the design point you type in yourself.

## A note on the design point

The band a single table can hold is roughly 2.6x wide in volume: at the shipped table, 85% share at
about $91K a day down to 65% at about $241K. Real games swing more than that, and a launch typically
runs several times settled volume before decaying, so a table tuned for settled conditions will
resolve early during launch. That is a genuine limitation of using one table rather than a flaw in
the numbers, and the honest answer is usually to pick the design point that maximises days inside
the band, which is not always the median you expect.
