# Personal BI Development

A portfolio of self-built Business Intelligence reports — data modeling, DAX,
and report design, all built and versioned the way I'd run it on a team.

## Stack

- **Power BI** — reports authored in **PBIP** (Power BI Project) format, so the
  semantic model (`.SemanticModel`) and report layer (`.Report`) are plain
  text/JSON and diff cleanly in git, instead of shipping as an opaque `.pbix`.

## Reports

| Report | Description |
| --- | --- |
| [2026 Monthly Expense Tracker](Power%20BI/2026_MonthlyExpenseTracker) | Personal expense tracking model and report for 2026 — categorized monthly spend with a semantic model driving the visuals. |

## Repository structure

```
Power BI/
  <ReportName>/
    <ReportName>.Report/          # Report layer: pages, visuals, layout
    <ReportName>.SemanticModel/   # Data model: tables, relationships, DAX
    <ReportName>.pbip             # Entry point — open this in Power BI Desktop
```

Each report is self-contained: open its `.pbip` file in Power BI Desktop to
load both the model and the report together.

## Data

All source/sample data in this repo is synthetic or personal, non-sensitive
data — nothing work-derived or proprietary is committed here (see
[PERSONAL-REPO-SETUP.md](PERSONAL-REPO-SETUP.md)).

## Contributing

This is a solo portfolio repo, but it's built and merged through the same
PR/branch discipline as a team project. See
[CONTRIBUTING.md](CONTRIBUTING.md) for the workflow.
