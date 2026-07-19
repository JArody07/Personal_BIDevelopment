# Scalable BI Repository — Setup & Collaboration Checklist

A reference for standing up (and auditing) this repository for multi-developer
collaboration. Work top to bottom for a fresh setup; use it as an audit list
before onboarding anyone new.

> **Audit these first — they fail *silently*.** If wrong, nothing errors; things
> just quietly don't work the way you assumed:
> - Org **base permissions** set to Read (not Write)
> - Project **auto-add workflow** enabled
> - **Labels** referenced by the forms actually exist in the repo
> - Branch protection **"Require review from Code Owners"** checked

---

## 1. Organization & access governance

- [ ] Repo lives under the **organization**, not a personal account.
- [ ] **Org Owners = you only.** (Org → People → filter by Owner.) The junior is
      a **Member**, never an Owner.
- [ ] Access is granted **through a team**, not per-person. The `bi-developers`
      team has **Write** on the repo.
- [ ] **No individual** is listed as a direct collaborator with Admin. (Repo →
      Settings → Collaborators and teams.)
- [ ] **Base permissions = Read** (or None). (Org → Settings → Member privileges.)
      *If this is Write, every member gets Write on every repo automatically,
      bypassing your team model.*
- [ ] **Require 2FA** for all org members. (Org → Settings → Authentication
      security.) Non-negotiable for a regulated shop.
- [ ] The junior is added to the **org itself** (so 2FA + team membership apply),
      then to the **team**.

**Why team-based:** new members *inherit* the team's access. You manage
permissions once on the team; membership is the only per-person lever. Add to
team → access granted everywhere. Remove from team → access revoked everywhere.

---

## 2. Repository protection (`main`)

Branch protection rule on `main`:

- [ ] **Require a pull request before merging.**
- [ ] Require **1 approving review**.
- [ ] **Require review from Code Owners.** *(This is what makes CODEOWNERS
      enforce, not just suggest.)*
- [ ] **Dismiss stale approvals when new commits are pushed.** *(A junior can't
      get approval, then quietly push more.)*
- [ ] **Require branches to be up to date before merging.** *(Reduces PBIP
      conflict surface.)*
- [ ] **Block force pushes** to `main`.
- [ ] `.github/CODEOWNERS` exists and routes critical paths to you:

```
*             @JRamirezTDA      # default owner
/Power-BI/    @JRamirezTDA
/SQL/         @JRamirezTDA
/CI-CD/       @JRamirezTDA
```

---

## 3. Merge hygiene

- [ ] **Squash merge = default** (and optionally disable merge-commit / rebase to
      remove the choice). Keeps `main` at one clean commit per ticket — valuable
      given PBIP's noisy JSON diffs.
- [ ] **Automatically delete head branches** after merge. (Repo → Settings →
      General → Pull Requests.) Keeps the branch list = active work only.

---

## 4. Intake system (Issues)

- [ ] `.github/ISSUE_TEMPLATE/` contains the two forms:
      `01_build_request.yml`, `02_bug_data_quality.yml`.
- [ ] `config.yml` with `blank_issues_enabled: false` **and** `contact_links`
      routing non-tickets (access requests, "which form?") away from the backlog.
- [ ] The labels the forms reference **exist** in the repo:
      `type: request`, `type: bug`, `needs-triage`
      *(names must match character-for-character, including the space after the
      colon, or auto-stamping silently no-ops).*
- [ ] Optional but recommended: **Priority** and **Department** as labels (or
      project fields), so the board can filter/group the way the old MD file was
      organized. (Form dropdowns land in the issue *body*, not on the card.)

**Verify:** visit `…/issues/new/choose` → exactly two form cards. A "Blank Issue
(for Maintainers only)" option is *expected* for you (elevated permissions); it
does not mean the config failed.

---

## 5. Project board

- [ ] **Org-level** project (not repo-level), set **Private**.
- [ ] Repos **linked** in project settings (so their issues are eligible).
- [ ] **Auto-add workflow** enabled: filter `is:issue`, target the repo.
      *(Without this, issues never appear on the board.)*
- [ ] Workflow: **Item closed → Done.**
- [ ] Workflow: **Pull request merged → Done.**
- [ ] Status columns match your flow: Backlog → Ready → In Progress → In Review
      → Blocked → Done.

---

## 6. Conventions (documented, not tooling-enforced)

- [ ] **Issue title:** `[Request]: <Area/Object> — <concise what>` /
      `[Bug]: <Area/Object> — <what's wrong>`. Lead with the object, not a verb.
      *(Title is often the only identifier visible on the board card.)*
- [ ] **Branch name:** `<type>/<issue#>-<short-slug>`
      (`report/`, `fix/`, `sql/`, `py/`). Created *from the issue* so the number
      and slug are pre-filled.
- [ ] **PR body:** `Closes #<issue>` — the real machine link that closes the
      issue and moves the card to Done. *(This matters more than the branch name.)*
- [ ] **One ticket → one branch → one PR.** No stacking unrelated work.
- [ ] **One report, one owner, one active branch at a time** (PBIP constraint,
      enforced via issue assignment).

---

## 7. Documentation

- [ ] `CONTRIBUTING.md` — the day-one document: issue→branch→PR→review→merge
      rhythm, branch/title naming, `Closes #`, the commit-*and*-push gotcha, the
      Fetch-first gotcha, PBIP one-owner rule.
- [ ] `README.md` intake rule — "no ticket, no work"; all work enters as a
      form-filed issue; triage cadence (e.g. Monday Backlog→Ready).
- [ ] This checklist, kept current as the setup evolves.

---

## 8. Compliance boundary (regulated environment)

- [ ] Written rule: **no CUI / ITAR-controlled technical data in issue or PR
      bodies.** Descriptions stay at the *process* level, never part numbers,
      specs, or drawings.
- [ ] Form placeholders steer requesters toward process-level description.
- [ ] Board / project is **Private**.

---

## 9. End-to-end verification (do this before the junior arrives)

Walk the whole loop once with a throwaway test issue. If every step fires, every
claim in your docs is verified:

- [ ] File a test issue through the **Build form** → confirm `type: request` and
      `needs-triage` auto-appear.
- [ ] Confirm it lands on the **board in Backlog** on its own (auto-add works).
- [ ] **Create a branch from the issue** → confirm it's named + linked.
- [ ] Push a trivial change → **open a PR with `Closes #<issue>`.**
- [ ] Confirm you (junior role) **cannot self-merge** until approved.
- [ ] Approve → **squash-merge.**
- [ ] Confirm: issue **auto-closed**, card moved to **Done**, branch
      **auto-deleted**.
- [ ] Delete the test issue.

---

## 10. Backlog migration (one-time)

- [ ] Convert existing MD tickets into issues. Hand-entry if ~10–20; script via
      `gh issue create` in a loop if 30+.
- [ ] Migrated issues are **backlog** — create issues only. **No branches** until
      someone actually starts each ticket.

---

## Why this matters for Data Engineering

This isn't just BI housekeeping — the practices you're standing up *are* the
software-engineering foundation DE hiring screens for. Frame them that way on
LinkedIn and in interviews:

- **Version control + PR review + branch protection** → you enforce a reviewed,
  auditable change process. This is baseline SWE rigor most BI/analytics people
  never develop; it transfers directly to dbt models, pipeline code, and IaC.
- **Issue-driven intake with required fields + templates** → reproducible,
  structured requirements capture. The same instinct behind schema contracts and
  data validation gates.
- **CODEOWNERS + governance for a small team** → you designed *asymmetric*
  access control (sole reviewer, team-based inheritance, least-privilege base
  perms). That's access governance, a real DE/platform concern.
- **CI-CD/ folder + squash-clean history** → you're building toward deployment
  automation, not just authoring reports. That trajectory (author → automate →
  govern) is the DE arc.
- **Compliance-aware metadata handling** → operating a change process inside
  ITAR/AS9100 boundaries is a rare, high-credibility differentiator. Most
  candidates can't speak to governance under regulation; you can.

The through-line for your narrative: *you didn't just build reports, you built
the engineered process around them.* That sentence is a resume bullet and a
LinkedIn post.

If you're reading this, this means I successfully cloned the repository and it's ready for cross-collaboration.