# Kurdistan_companies_analysing

# Stamped & Registered — Kurdistan's Business Registry, Decoded

A data-driven look at 15,541 registered companies in the Kurdistan Region of
Iraq, built from a filing-level extract of the official business registry.
This repo contains the underlying dataset, the analysis/report, and the
Python script used to generate every chart in it.

---

## Table of Contents

- [About the Report](#about-the-report)
- [Key Findings](#key-findings)
- [Dataset](#dataset)
- [Repository Structure](#repository-structure)
- [Getting Started](#getting-started)
- [Charts Generated](#charts-generated)
- [Methodology & Notes](#methodology--notes)
- [Limitations](#limitations)
- [Sources & External Context](#sources--external-context)
- [License](#license)

---

## About the Report

**Stamped & Registered** is an independent analysis of the Kurdistan Region
business registry — not an official government publication. It pulls apart
what 15,541 company filings, spanning **1998 to August 2026**, reveal about
how business actually gets done in the region: what people register
companies to do, where they register them, how much capital they declare,
who runs them, and how registration activity has changed over nearly three
decades.

The report is organized around a few core questions:

- **What kind of businesses are being registered?** (dominated overwhelmingly
  by trading and commerce)
- **Where are they registering?** (heavily concentrated in Erbil and
  Sulaymaniyah)
- **When did registration activity happen?** (a slow, steady climb for
  twenty years, followed by a sharp surge starting in 2023)
- **Who's behind these companies?** (CEO gender split, legal structure)
- **How much capital is actually being declared?** (a small number of
  filings account for a hugely disproportionate share of declared capital)

Along the way it compares Kurdistan's numbers against global and regional
benchmarks — ILO workforce statistics, U.S. small-business data, and
neighboring economies — to put the region's heavy tilt toward trading in
context.

---

## Key Findings

- **Trading dominates.** Roughly **63%** of all registered companies fall
  under trading & commerce activities — several times higher than the
  global average share of the workforce in wholesale & retail trade (~12–18%
  per ILO and U.S. benchmarks).
- **General Trading is the single most common license**, held by thousands
  of companies — far ahead of the second most common activity, building
  contracting.
- **Registrations have exploded in recent years.** After two decades of
  gradual growth, new filings roughly tripled between 2022 and 2023, with
  2023–2025 each producing well over 2,000 new companies a year — more than
  in nearly the entire 2000s combined.
- **Erbil and Sulaymaniyah account for the large majority of companies**,
  with the remaining provinces (Duhok, Zakho, Raparin, Garmian, Soran,
  Halabja) making up a much smaller share.
- **Capital declarations are extremely unequal.** The median declared
  capital sits at a token amount, while the top 1% of filings declare
  capital worth thousands of times more — a small number of companies
  account for a disproportionate share of registered capital in the
  dataset.
- **Company leadership skews heavily male.** The overwhelming majority of
  single-CEO companies list a male CEO, with female-led companies making up
  a small minority.
- **Private Limited companies are the default structure**, used by over 90%
  of all registered businesses.
- Beyond the headline charts, the report also surfaces some standout
  one-off facts — companies licensed for over 100 separate activities on a
  single filing, licensed currency-exchange intermediaries, and building
  cleaning services outnumbering IT consultancies by roughly two to one.

---

## Dataset

**File:** `kurdistan_companies_combined.csv`
**Rows:** 15,541 companies
**Coverage:** Erbil, Sulaymaniyah, Duhok, Zakho, Raparin, Garmian, Soran, Halabja
**Date range:** June 1998 – August 6, 2026

| Column                        | Description                                             |
|--------------------------------|-----------------------------------------------------------|
| `companyId`                    | Unique internal identifier                                |
| `uniqueEntityNumber`           | Official registry entity number                           |
| `companyName`                  | Registered company name                                   |
| `ceoName`                      | Name(s) of CEO(s) on file                                  |
| `ceoGender`                    | Gender of CEO(s); multiple values separated by `;` for joint leadership |
| `establishedDate`              | Date the company was registered                            |
| `companyType`                  | Legal structure (Private Limited, Individual Project, etc.) |
| `companyOrigin`                | Local vs. foreign company                                  |
| `province`, `district`, `street`, `neighborhood` | Registered location                    |
| `activityTypes`                | Licensed business activities, `;`-separated                |
| `capital`                      | Declared capital (converted to USD)                        |
| `shareholderNames`             | Shareholder(s) on file                                     |
| `shareholderGender`            | Shareholder gender(s)                                      |
| `shareholderNationality`       | Shareholder nationality                                     |
| `shareholderNumberOfShares`    | Number of shares held                                       |
| `shareholderPercentage`        | Ownership percentage                                         |
| `shareholderIdType`            | ID type used for shareholder registration                   |

Capital figures are converted at approximately **1,310 IQD/USD** (August
2026 rate). Percentages throughout the report are of this 15,541-company
extract, not of every business that has ever existed in the region.

---

## Repository Structure

```
.
├── kurdistan_companies_combined.csv   # Raw registry data (15,541 rows)
├── make_charts.py                     # Generates all report charts from the CSV
├── charts/                            # Output folder — PNGs are written here
└── README.md                          # This file
```

---

## Getting Started

### Requirements

- Python 3.8+
- pandas
- matplotlib

Install dependencies:

```bash
pip install pandas matplotlib
```

### Run

Make sure `kurdistan_companies_combined.csv` is in the same folder as the
script, then:

```bash
python make_charts.py
```

All charts are saved as PNG files into a `charts/` folder.

---

## Charts Generated

| # | File                                         | Description                                                  |
|---|-----------------------------------------------|----------------------------------------------------------------|
| 1 | `01_top_activities.png`                      | Top 10 licensed business activities by number of companies    |
| 2 | `02_categories.png`                          | Companies grouped into broad activity categories (% of total) |
| 3 | `03_yearly_filings.png`                      | New filings per year, plus cumulative total over time         |
| 4a| `04a_province_counts.png`                    | Number of companies registered per province                   |
| 4b| `04b_province_general_trading_share.png`     | Share of "General Trading" licenses within each province      |
| 5a| `05a_ceo_gender.png`                         | CEO gender split (single-CEO companies only)                  |
| 5b| `05b_company_type.png`                       | Companies by legal structure (%)                                |
| 6 | `06_capital_percentiles.png`                 | Declared capital by percentile, log scale                      |

---

## Methodology & Notes

- **Activity categorization** (chart 2) groups the raw `activityTypes`
  field into broader buckets using keyword matching (e.g. anything
  containing "trad", "wholesale", or "retail" is counted under Trading &
  Commerce). Adjust the `category_keywords` dictionary in `make_charts.py`
  to change these groupings.
- **Capital percentiles** (chart 6) are plotted on a **log scale** because
  the top percentiles are on the order of thousands of times larger than
  the median — a linear scale would flatten the lower bars into
  invisibility. The real dollar value for each percentile is printed
  directly next to its bar.
- **CEO gender** (chart 5a) only counts companies with a single CEO listed
  as strictly `male` or `female`. Joint filings with multiple CEOs (e.g.
  `"male; male"`) are excluded from this chart, consistent with how the
  report treats single-CEO companies.
- **Repeated CEO names are not confirmed to be the same individual** — no
  entity resolution was performed across rows.
- Large capital outliers are flagged in the analysis, not excluded from the
  dataset or charts.

---

## Limitations

- The dataset is a snapshot extract of the registry, not necessarily a
  complete historical record of every business ever registered in the
  region — companies that have since closed, merged, or been struck off
  may or may not be included depending on how the extract was compiled.
- Activity categorization is keyword-based and approximate; some companies
  may be miscategorized or fall into more than one bucket.
- Currency conversion uses a single fixed exchange rate (~1,310 IQD/USD),
  which does not account for historical exchange rate fluctuations for
  companies registered in earlier years.
- Gender and demographic figures reflect what is listed on official
  filings only.

---

## Sources & External Context

The report draws on the following external sources for comparison and
context:

- American-Kurdish Institute for Economic Research
- Wikipedia — Economy of the Kurdistan Region
- GOV.UK — Doing Business in Iraq trade guide
- GOV.KRD — National Company Registration
- Kurdistan Gate & Global Law Experts, company law guides
- Al Jazeera & AFP reporting on Kurdish women entrepreneurs
- ILO & U.S. small-business statistics, used for international comparison

---

## License

This project is provided for research and educational purposes. Add your
preferred license (e.g. MIT) here before publishing.
