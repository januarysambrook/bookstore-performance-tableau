# Bookstore Performance, Tableau

A Tableau analysis of bookstore sales and profitability, built as an assessed final
project. The analytical work is in the data model: three monthly sales files have to be
unioned and then related to a separate sales reference file whose keys do not match.

## The problem worth reading about

The three first-quarter 2024 files union cleanly, because they share a structure. The
external `Sales.csv` file does not join cleanly, and the reason is subtle.

Both sides carry a field called `Item Name`. The names match. The **values** do not:
the key in the unioned files is formatted differently from the foreign key in the
external file. A join on identically named columns therefore returns nothing, or worse,
silently duplicates rows where partial matches occur.

The fix is to relate the tables rather than join them, so Tableau resolves the
relationship at the level each visual needs instead of flattening everything into one
table first. That avoids the double counting a naive join produces. The figures in the
finished workbook were verified as correct, including the History category landing at
$12,000 or under, which is the value a double-counting model gets wrong.

This is the part of the project I would talk about in an interview. Matching column
names are not matching keys, and a join that returns plausible-looking numbers is more
dangerous than one that returns none.

## What the analysis covers

- Sales and profitability across categories, with a cross-tabular heat map shading cell
  backgrounds rather than text, so the figures stay readable
- Stacked bar charts breaking revenue down by dimension
- Regional performance comparison
- A dashboard bringing four visuals together with a filter, sized to fit automatically

## Assessment

Submitted as the Tableau final project and marked **81 / 100**. The trainer's comments
recorded the data model as correctly built and the figures as correct, and described the
presentation as the strongest part of the submission.

Three things were marked down, and they are worth recording honestly:

- **The data model screenshots were missing.** The submission required evidence of the
  Data Source tab showing how the union and the relationship were set up. The figures
  proved the model worked, but the evidence itself was absent.
- **Currency symbols were missing from axis titles and value labels.** The dataset is a
  US retailer, so a dollar prepend belongs on every figure. It was stated in the written
  analysis but not carried into the visuals.
- **The commentary was descriptive rather than prescriptive.** Each visual carried an
  explanation of what the chart showed, but not enough on what the business should do
  about it.

## Data

Three monthly bookstore sales files for January, February and March 2024, unioned, plus
an external sales reference file related on item name.

## What I would do differently

Add the model screenshots, put the currency symbol on every axis and label, and make the
commentary prescriptive throughout rather than only on the closing slide. All three are
presentation discipline rather than analysis, which is why the data model and the figures
scored well while the write-up lost marks.

---

Built by January Sambrook
