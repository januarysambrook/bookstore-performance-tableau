# Q1 2024 Bookstore Performance, Tableau

A quarterly performance review for a national bookstore group selling across four US
regions, built in Tableau to direct marketing spend and purchasing for Q2.

**750 orders, $73,792.73 in sales, 1 January to 28 March 2024.**

> The West is 49.7% of revenue, more than the other three regions combined, and the gap
> is volume rather than value: 360 orders against the South's 80, while average order
> value varies by only 15%. The South's problem is reach, not pricing.

## The join that would have broken the analysis

This is the part of the project worth reading.

The brief specified joining the unioned Q1 sales to `Bookstore_Sales.csv` on
`PublisherID`. That table holds roughly 12 rows per publisher and spans 2020 to 2024, so
it is a **different grain and a different period** from a Q1 2024 order table.

Executed as a physical inner or left join, the 750-row fact table fans out to **8,704
rows**, and total sales inflate from $73,792.73 to **$875,069.56**. That is an **11.86x
overstatement**, and nothing about the output looks obviously broken. Every chart would
still render. Every number would be wrong.

The fix was to connect it as a Tableau **relationship on the logical layer** rather than
a physical join, so each table aggregates at its own level and the Q1 totals stay
correct.

A join that returns plausible numbers is more dangerous than one that returns none.
Matching key names tell you nothing about matching grain.

## Findings

### Basket size is the lever, not genre

Large orders of five or more units are 41% of rows but **61.7% of revenue**, $45,509 of
the quarter. Small orders of one or two units are 29% of rows and only 11.6% of revenue.

By contrast, genre barely separates. History leads at $12,317, 16.7% of sales, and
Children trails at $6,887, 9.3%. Only 7.4 percentage points separate best from worst. No
genre is failing and none is dominant.

### The regional gap is reach, not price

| Region | Sales | Share | Orders | Avg per order |
| --- | ---: | ---: | ---: | ---: |
| West | $36,666.66 | 49.7% | 360 | $101.85 |
| East | $17,056.30 | 23.1% | 174 | $98.02 |
| Central | $13,106.97 | 17.8% | 136 | $96.37 |
| South | $6,962.80 | 9.4% | 80 | $87.04 |

Total sales differ by 5.3 times across regions while average order value differs by only
15%. That is a volume story. The South is not selling cheaply, it is barely selling.

The genre-by-region highlight table sharpens it: the hot cell is West and History at
$6,287, the cold cell is South and Non-Fiction at $156, a 40x spread. The West is strong
in **every** genre without exception, which points to distribution or store footprint
rather than merchandising. The South has no genre it performs well in, so there is no
niche to build a Q2 action on.

### Supplier risk is low, and so is leverage

Revenue is fragmented across 60 publishers, with the top ten accounting for only about
24% of sales. Publisher_013 leads at $2,171 and Publisher_058 trails at $328. The top
five sit within 0.6 percentage points of each other, so the leaderboard is unstable and
could reorder on one good quarter. No partner is critical, which cuts risk but also
means none has meaningful negotiating weight.

### The quarter was stable and finished up

Monthly sales held at $24,387, $23,414 and $25,991. Weekly sales ran between $5,100 and
$7,300, with the strongest week beginning 18 March at $7,274.

Two apparent troughs are **artefacts, not declines**: each monthly file stops on the
28th, so the weeks beginning 29 January and 25 March are partial. Reading them as a
downturn would have been the easy mistake.

## Data quality

Checked and documented rather than assumed:

- **Missing values: none.** All ten fields across 750 rows fully populated.
- **Duplicate rows: none.**
- **Duplicate OrderID: one.** `BM-03-493700` appears twice with different dates,
  publishers, genres and values. That is an ID collision, not a duplicated sale, so both
  rows were retained and the caveat noted against any distinct count.
- **Outliers: four flagged, all retained.** Each is a legitimate seven-unit order at a
  high unit price, inside the valid ranges of both fields. Removing them would understate
  the top of the market.
- **Derived field verified.** Sales equals Quantity multiplied by UnitPrice on all 750
  rows, with no mismatches.

## Model

- Three monthly files with an identical ten-column schema, **unioned** into one 750-row
  Q1 fact table
- `Publisher_Info.csv`, exactly one row per PublisherID, joined as a **left join** so no
  sales row can be dropped if a publisher were ever missing from the lookup
- `Bookstore_Sales.csv` connected as a **relationship**, for the reason above

Calculated fields, built only from supplied columns as the brief required:

```
Avg. Sales per Order    = SUM([Sales]) / COUNTD([OrderID])        -> $98.52
Avg. Unit Price (wtd)   = SUM([Sales]) / SUM([Quantity])          -> $24.86
Order Size              = IF [Quantity] >= 5 THEN "Large (5+)" ...
```

The weighted unit price matters: averaging list prices would have given the catalogue
average rather than the price customers actually paid.

## A deliberate deviation from the brief

Task 4 specified a line chart with Genre on colour. Eight genre lines across thirteen
weeks produced an unreadable chart, so a stacked bar was substituted: total weekly sales
stay legible as bar height while genre composition is preserved as colour. The deviation
and the reason are stated on the slide rather than hidden.

## Assessment

Marked **81 / 100**. The data model and the figures were recorded as correct, and the
presentation was described as the strongest part of the submission.

Three deductions, recorded honestly:

- **Data Source tab screenshots were missing.** The figures proved the model worked, but
  the required evidence of the union and the relationship was absent.
- **Currency symbols were missing from axis titles and value labels** on a US dataset.
- **The commentary was descriptive rather than prescriptive,** explaining what each chart
  showed without consistently saying what the business should do about it.

## Limitations

- **One quarter, no prior year.** Nothing here establishes a trend.
- **United States only,** across 10 states and 48 cities, so no international comparison
  is possible.
- **No customer field,** so repeat purchase and customer value cannot be measured.
- **No cost or discount data,** so this is a sales analysis, not a profitability one.
- **Partial final weeks,** since each monthly file ends on the 28th.

---

Built by January Sambrook
