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

## Use of Claude

After successfully installing claude, make sure you use the following to reduce downtime related to explaining things to Claude over and over again.

> Create and Use The Following

What is [Claude.md](/Users/jarody07/Repositories/Personal/Personal_BIDevelopment/CLAUDE.md)?

A plain text Markdown document placed in the root directory of a repo. It acts as a persistent memory and onboarding manual for Claude Code, automatically loading project rules, tech stacks, and commands at the start of every session so you never have to re-explain them.

Your [Claude.md](/Users/jarody07/Repositories/Personal/Personal_BIDevelopment/CLAUDE.md) typically follows the following structure:

#### What Goes in a CLAUDE.md File:

`- Project Overview: A brief 2-to-3 sentence summary of what the 
project does and its target audience.`

- Tech Stack: Explicit lists of frameworks, languages, and core
libraries being used (e.g., TypeScript, Next.js, Tailwind).

- Development Commands: Exact terminal instructions for building,
running tests, linting, or starting the dev server.

- Coding Standards: Preferred patterns, architectural layout, naming
conventions, and constraints (e.g., use server components, prefer
named exports).

#### What goes in .claude/rules/

Here you can actually enforce these files, in the context of preventing Claude from modifying certain files this is your go to. Avoid relying on [.claudeignore](/Users/jarody07/Repositories/Personal/Personal_BIDevelopment/.claudeignore) it for secrets, Claude usually tells users to put .env files into .claudeignore to protect secrets, but native support is inconsistent because it acts primarily as a context filter rather than a rigid sandbox barrier, Claude can still accidentally read files matching your ignore patterns if its sub-agents or shell tools explicitly look for them.

In order to provide Claude with context, with things such as business logic or posting personas, you can also use Claude rules to do so. For example, we can create something like .claude/rules/linkedin-voice.md alongside business-logic.md and schemas.md, then add a reference line to it in CLAUDE.md's "Conventions" or a new "Content" section.
