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
- **`.tmdl`/`.json` files are read-only, no exceptions** — never Edit or Write them, even if asked to hand-edit. These are Power BI's generated project format; all model/report changes must be made in Power BI Desktop and let it regenerate the files. (Enforced in `.claude/settings.json`.)
- `LocalDateTable_*.tmdl` and `DateTableTemplate_*.tmdl` files are Power BI's auto-generated hidden date tables — not user-authored, ignore when reasoning about the model.
- `cultures/en-US.tmdl` is auto-generated translation metadata — very large, not useful context.

## Collaboration

This repo is also how I'm learning, not just a report I'm shipping — see [.claude/rules/collaboration-style.md](.claude/rules/collaboration-style.md) for how guidance should differ between areas I'm actively learning and territory I already direct precisely.

## Content

This repo doubles as portfolio/learning content — I post about progress on LinkedIn. When asked to draft a post, pull the narrative from whatever's actually being worked on (recent commits, current session context) unless I hand you a specific topic. For voice/tone/structure rules, see [.claude/rules/linkedin-voice.md](.claude/rules/linkedin-voice.md).

<!-- Repo narrative notes go here as they develop — e.g. why this project exists, what "done" looks like, milestones worth posting about. -->
