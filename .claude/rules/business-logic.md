# Business Logic — 2026 Monthly Expense Tracker

Metric definitions and domain rules, derived from the DAX in `Fact_BankTransactions.tmdl` and `Budget_2026.tmdl`. This file loads every session (no `paths` scoping) since these definitions apply across the report, not just when editing the model.

<!-- TODO (user): the bucket names and measure formulas below are read directly from the
     DAX — accurate as of when this was written, but the *why* behind them (e.g. what
     counts as "Needs" vs "Wants", why Savings is excluded from Net Cash Flow) isn't
     encoded anywhere. Fill that in here so Claude doesn't have to guess or ask each time. -->

## Budget Framework: Needs / Wants / Savings

Every category in `DimCategory` is tagged with a `FinancePlanCategory`, one of:

- **Needs** — essential spending
- **Wants** — discretionary spending
- **Savings** — money moved to savings
- **Income** — money coming in (excluded from expense totals)
- **Internal Transfer** — moves between own accounts (excluded from expense totals — not real spending)

`[Needs %]`, `[Wants %]`, `[Savings %]` measures divide each bucket's total by `[Total Expenses]` — this is the model's version of a 50/30/20-style budget split. <!-- TODO: confirm target percentages if there are any -->

## Core Measures (Fact_BankTransactions)

- **Total Transactions** = `SUM(Transaction Amount)` — the raw signed sum; base measure most others build on.
- **Total Expenses** = Total Transactions excluding `Internal Transfer` and `Income` categories. Still includes Savings.
- **Total Expenses (Excl. Savings)** = Total Expenses further excluding `Savings` — used for Net Cash Flow.
- **Total Income** = Total Transactions where category = `Income`.
- **Net Cash Flow** = Total Income + Total Expenses (Excl. Savings). *(Expenses are stored as negative amounts, so this is addition, not subtraction.)*
- **Total Expenses (Display)** / **Total Expenses (Excl. Savings) (Display)** = `ABS()` wrapped versions for display in visuals (so expenses render as positive numbers).
- **Running Balance** — per `AccountSource`, starting balance from `StartingBalances` plus cumulative transactions up to the selected date.
- **Budget Variance** = `[Monthly Budget] + [Total Expenses]` (Budget is positive, Expenses negative — so this nets to over/under).
- **Current/Previous/Benchmark Month Expenses & Variance** — same expense/variance logic pinned to a specific month: current calendar month, prior calendar month, or the month selected via `BenchmarkMonthSelector`.
- **Total Expenses or Income** — context-sensitive: returns Income when the visual is sliced to the Income category, Expenses otherwise (lets one visual show both without double-counting).

## Amazon Order Matching

Orders are auto-categorized via `AmazonCategoryMap` keyword rules (`Priority` field controls rule precedence — lower/higher priority wins, <!-- TODO: confirm which -->), then matched against bank transactions in `Amazon_BankMatch`. Purpose: reconcile the generic "Amazon" bank-statement line item down to what was actually purchased.

## Manual Category Overrides

`CategoryOverrides` lets a transaction's auto-assigned category be corrected after the fact; `Fact_BankTransactions` reflects overrides applied, `Fact_BankTransactions_PreOverride` is the raw pre-override version for auditing.
