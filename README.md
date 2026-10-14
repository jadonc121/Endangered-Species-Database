# U.S. Endangered Species: SQL Exploration

_**Early project (2023).** This was a SQL practice project written as one notebook before I moved into industry. I've kept it to show how I started. I now build projects as modular Python code with reproducible data pipelines. See [my current project](https://github.com/jadonc121/causal-inference) for what that looks like._

Exploratory analysis of ~63,000 at-risk U.S. species from NatureServe Explorer, done mostly in PostgreSQL queries run from a Jupyter notebook, with Python for the charts.

**Notebook:** [U.S. Endangered Species Database.ipynb](U.S.%20Endangered%20Species%20Database.ipynb)

## Questions
- How many species are there, and how are they classified into groups and subgroups?
- How are species ranked by level of endangerment (NatureServe global ranks, G1 to GX)?
- Which groups have the most secure, extinct, or unranked species?
- How are species distributed across regions (U.S. vs. Canada, by state)?

## Data
NatureServe Explorer (https://explorer.natureserve.org/) data on at-risk species in North America, downloaded, cleaned, and loaded into a local PostgreSQL database (63,268 species). The database is local and is not included in this repository.

## Approach
- Connected a notebook to PostgreSQL with `ipython-sql` for inline queries, and SQLAlchemy + pandas to pull results into Python for plotting.
- Wrote reusable SQL patterns: common table expressions (CTEs) for counts and totals, `COUNT(*) FILTER (WHERE ...)` for conditional aggregation, `STRING_AGG`, regular-expression search, subqueries, and percentage calculations with explicit casts.
- Handled data-quality issues explicitly, such as species with no subgroup, and ranked vs. unranked species (aggregates over "secure" and "extinct" use only ranked species, to avoid misleading percentages).

## Selected findings
- **63,268** species in the database; **31.6%** are flowering plants, the largest group, followed by butterflies and moths (10.5%) and beetles (8.6%).
- **40.8%** (25,841) of species are unranked (GNR, GNA, or GU). This is a data-coverage gap worth flagging before drawing any conclusions about risk.
- Among the **37,427 ranked** species, **42.7%** are secure (G5) and **0.5%** (189) are presumed extinct (GX).
- **57.5%** of species occur only in the U.S.; **42.5%** also have habitat in Canada.
- **Colorado:** 6,653 species, 10.5% of the total.
- Within-group views show different patterns than raw counts, for example insect groups are heavily unranked, which suggests data limitations and not low risk. (Explanations in the notebook are hypotheses, not tested.)

## Skills demonstrated
SQL (PostgreSQL): CTEs, conditional aggregation, regex filtering, string aggregation; Python (pandas, SQLAlchemy, matplotlib, seaborn); exploratory data analysis; data quality handling; communicating findings from queries.

## How to run
Requires PostgreSQL, Python 3, and `pandas`, `matplotlib`, `seaborn`, `sqlalchemy`, `ipython-sql`. Load the NatureServe data into a local PostgreSQL database and point the notebook's connection at it.

## What I'd do differently today
Move the SQL into versioned `.sql` files, add a reproducible data-loading script (or a Docker Compose Postgres), replace pie charts with sorted bar charts, and add tests for the data-cleaning steps.
