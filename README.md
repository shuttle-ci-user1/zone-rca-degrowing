# zone-rca-degrowing

Zone RCA dashboard for degrowing zones — zone OPD, zone-vs-city comparison,
WOW/MOM trends, and L4W/WTD/MTD views.

> **Note:** `swiggy-private/zone-rca-degrowing` (the configured source repo)
> does not exist / is not accessible to the CI account. This repo carries a
> fresh self-contained implementation. See
> [MISSING-SOURCE.md](MISSING-SOURCE.md) for the full search that was done.

## Views

| View | What it shows |
| --- | --- |
| Summary comparison | One row per zone; click a row to **expand it in place** (WOW trend, MOM trend, zone-vs-city, top RCA buckets) without scrolling through zone cards |
| WOW trend | Last 5 weeks of zone OPD with sparkbars, WoW delta, growing/flat/degrowing status |
| MOM trend | Same comparable layout over the last 5 months, with MoM delta |
| L4W / WTD / MTD | Zone OPD **and** zone-vs-city comparison (city OPD, zone share, vs even-share expectation) always visible, plus zone cards |

## Usage

Open `index.html` directly — it is a single self-contained file with embedded
sample data. To refresh with real data, replace the `DATA` array at the top of
the `<script>` block with the weekly RCA pipeline output (same shape:
`weeklyOpd[5]`, `monthlyOpd[5]`, `rca` buckets).

Status classification: latest period delta < −1% = DEGROWING, > +1% = GROWING,
otherwise FLAT.
