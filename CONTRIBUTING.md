# Scalable BI Repository — Setup & Collaboration Checklist

The purpose of this repository is primarily for personal projects.

## Contribution Workflow

- Cloning the Repo:

- Branching:

  - `main` is protected — no direct commits or pushes. All changes go through a branch and a PR.
  - Branch off the latest `main`: `git checkout main && git pull && git checkout -b <type>/<short-description>`.
  - Branch prefixes: `feature/` for new functionality, `fix/` for bug fixes, `chore/` for maintenance/tooling (e.g. `feature/monthly-comparison-table`, `fix/relationship-cardinality`).
  - Keep branches scoped to one change and short-lived — merge or close before starting the next one.

- Git Best Practices:

  - Every change lands via a pull request, even solo — it keeps a reviewable diff and a clean history on `main`.
  - Write commit messages that explain *why* a change was made, not just what changed.
  - Rebase your branch on `main` before opening/merging a PR if `main` has moved on.
  - Delete the branch once its PR is merged.