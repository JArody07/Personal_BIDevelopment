---
paths:
  - "Power BI/**/*.tmdl"
  - "Power BI/**/*.json"
---

# Semantic Model — 2026 Monthly Expense Tracker

Curated summary of the model, derived from the TMDL. Update this when tables/relationships change — don't rely on Claude re-deriving structure from the raw TMDL (the culture file alone is 20k+ lines).

## Core Tables

**Fact_BankTransactions** — the primary fact table. Bank transactions after category overrides are applied.
- `Transaction Date` (dateTime), `Transaction Description` (string), `Transaction Amount` (double), `AccountSource` (string), `Category` (string), `PayPeriod` (string)
- Holds most of the model's measures (see business-logic.md)

**Fact_BankTransactions_PreOverride** — same shape as `Fact_BankTransactions`, before manual category overrides. Reference/audit table.

**CategoryOverrides** — manual corrections to a transaction's category. `Category`, `Transaction Date`, `Transaction Description`, `Transaction Amount`.

**DimCategory** — category dimension. `Category` (string), `FinancePlanCategory` (string — the Needs/Wants/Savings/Income/Internal Transfer bucket; see business-logic.md).

**DimDate** — standard date dimension: `Date`, `DateKey`, `Year`, `Quarter`/`QuarterNumber`, `Month*`, `Week*`, `IsWeekend`, `IsToday`, etc.

**DimPayPeriod** — `PayPeriod` (string). Groups transactions by pay period.

**Budget_2026** — `Category`, `Amount` (decimal, the budgeted amount), `% Occupancy`. Marked "reference only" in the model — not a fact table to join transactions against directly.

**Gross_Income** — `Month`, `Transaction_Date`, `Income` (decimal).

**StartingBalances** — `AccountSource`, `OpeningBalance`, `AsOfDate`. Used as the seed for the running-balance calculation.

**BenchmarkMonthSelector** — calculated table, `YearMonth`/`MonthStart`. Drives the "Benchmark Month" comparison measures — lets the report compare current spend to a user-selected past month.

## Amazon Order Matching

A subsystem that reconciles Amazon orders against bank transactions:
- **Amazon_StagingQuery** → raw Amazon order export (`Order ID`, `Order Date`, `Total Amount`, item titles, `ItemCount`)
- **AmazonCategoryMap** — `Keyword`, `Category`, `Priority`: keyword-based rules to auto-categorize Amazon orders
- **Amazon_CategorizedOrders** — staging output with `Category` applied
- **Amazon_BankMatch** — matches categorized Amazon orders to bank transactions (`MatchCount`, `MatchedOrderIDs`)

## Relationships

- `DimDate` fans out to five hidden `LocalDateTable_*` auto-date tables (one per date-typed column used in a visual) — these are Power BI-generated, not part of the intentional model design.
- `Fact_BankTransactions['Transaction Date']` → `DimDate.Date`
- `Fact_BankTransactions.Category` → `DimCategory.Category`
- `Fact_BankTransactions.PayPeriod` → `DimPayPeriod.PayPeriod`
- `Budget_2026.Category` → `DimCategory.Category` (bothDirections, one-to-many)
- `Gross_Income.Transaction_Date` → `DimDate.Date`
- `StartingBalances.AsOfDate`, `CategoryOverrides['Transaction Date']`, `Amazon_*['Order Date'/'Transaction Date']`, `BenchmarkMonthSelector.MonthStart` each join to their own hidden local date table.

## Auto-Generated / Ignore

- `LocalDateTable_*.tmdl`, `DateTableTemplate_*.tmdl` — Power BI's automatic date hierarchy tables, one per date column with auto date/time enabled. Not hand-authored.
- `cultures/en-US.tmdl` — translation/formatting metadata, auto-generated, ~20k lines. Never needed for reasoning about the model.
