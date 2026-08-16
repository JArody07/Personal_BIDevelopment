# Personal_BIDevelopment

Personal BI repository. Primary project: **2026 Monthly Expense Tracker**, a Power BI report tracking personal bank transactions, budget, and income against a Needs/Wants/Savings plan.

## Tech Stack

- **Power BI Desktop**, saved in **PBIP** (Power BI Project) format — not `.pbix`. Report and semantic model are split into folders with `.Report` and `.SemanticModel` suffixes.
- Semantic model defined in **TMDL** (Tabular Model Definition Language) under `*.SemanticModel/definition/`.
- Report pages/visuals defined in JSON under `*.Report/definition/`.
- Data transformation via **Power Query (M)** — see `partition ... = m` blocks in TMDL table files.
- Measures written in **DAX**.

## Repo Layout

- `Power BI/<ProjectName>/<ProjectName>.SemanticModel/definition/` — model.tmdl, relationships.tmdl, expressions.tmdl, `tables/*.tmdl`
- `Power BI/<ProjectName>/<ProjectName>.Report/definition/` — report.json, `pages/*/page.json`
- See [.claude/rules/schemas.md](.claude/rules/schemas.md) for the semantic model structure and [.claude/rules/business-logic.md](.claude/rules/business-logic.md) for metric definitions.

## Conventions

- `main` is protected — no direct commits. Branch and open a PR for every change (see `CONTRIBUTING.md`).
- Branch prefixes: `feature/`, `fix/`, `chore/`.
- **Read access to `.tmdl`/`.json` files is fine; do not Edit/Write them directly** — these are Power BI's generated project format. Make model/report changes in Power BI Desktop and let it regenerate the files, unless explicitly asked to hand-edit TMDL/JSON. (Enforced in `.claude/settings.json`.)
- `LocalDateTable_*.tmdl` and `DateTableTemplate_*.tmdl` files are Power BI's auto-generated hidden date tables — not user-authored, ignore when reasoning about the model.
- `cultures/en-US.tmdl` is auto-generated translation metadata — very large, not useful context.
