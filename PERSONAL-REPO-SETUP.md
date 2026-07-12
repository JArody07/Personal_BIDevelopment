# Personal Repo — Setup & Hygiene Checklist

The personal-account counterpart to the work `CONTRIBUTING.md`. That document is
about *access governance for other humans in a regulated shop*. This one drops
all of that and keeps only what still earns its place solo: **PR + CI
discipline, clean history, secret hygiene, and public-brand signal.**

> **The one rule that inverts:** at work, compliance keeps controlled data
> *inside* the repo. Here, the risk runs the other way — keeping anything
> work-derived *out* of a public repo under your name. That rule is Section 5,
> and it matters more than every branch setting combined.

---

## 0. The fulcrum decision: public vs. private

On a **free personal account**, branch protection / rulesets work on **public**
repos and are **disabled on private** ones (private needs Pro, ~$4/mo).

- **Portfolio / learning repos → public + protected.** You get the full toolkit
  for free on exactly the repos where it matters, *and* public CI is your best
  DE signal.
- **Genuinely private junk (experiments, real data) → private, unprotected.**
  Don't pay to govern a repo only you touch and no one sees.

Everything below assumes a **public** portfolio repo unless noted.

---

## 1. Account-level (do once, applies everywhere)

- [ ] **2FA on.** Non-negotiable, same as work.
- [ ] **Profile README** (`<username>/<username>` repo) — your landing page.
- [ ] **Pinned repos** curated to your best 3–6. Recruiters read these first.
- [ ] Secret scanning + **push protection** default-on for public repos —
      confirm it's enabled (Settings → Code security).

---

## 2. Per-repo setup (each new repo)

- [ ] **`LICENSE`** — MIT or Apache-2.0. Without one it isn't legally reusable,
      so it isn't actually open source.
- [ ] **`.gitignore`** — language-appropriate; keep venvs, `.env`, data dumps,
      and OS cruft out. (You already keep repos outside iCloud — same instinct.)
- [ ] **`README.md` as the front page, not internal docs.** What it does, why,
      the stack, and a result/screenshot up top. This is the 15-second recruiter
      read — invest here. Skip a separate `CONTRIBUTING.md` unless the repo
      actually attracts outside contributors.
- [ ] **Import the ruleset** (`personal-main-ruleset.json`):
      Settings → Rules → Rulesets → New ruleset → **Import a ruleset**.
- [ ] **Add the CI workflow** (`.github/workflows/ci.yml`).
- [ ] After the first CI run, confirm the **`test`** check appears, then
      double-check the ruleset lists it as required.

---

## 3. Merge hygiene (keep entirely — free polish)

- [ ] **Squash merge = default**; optionally disable merge-commit / rebase.
      One clean commit per ticket on `main`.
- [ ] **Auto-delete head branches** after merge (Settings → General → Pull
      Requests). Branch list = active work only.

---

## 4. Conventions (kept — this is the transferable muscle memory)

Same rhythm you'll reuse on dbt models and pipeline PRs:

- [ ] **Branch:** `<type>/<issue#>-<slug>` — `feat/`, `fix/`, `sql/`, `py/`,
      `docs/`. Create it from the issue when you use issues.
- [ ] **PR body:** `Closes #<issue>` — the machine link that closes + moves the
      card.
- [ ] **One ticket → one branch → one PR.** No stacking unrelated work.
- [ ] Commit messages in the imperative ("Add …", "Fix …") — conventional
      enough to skim.

*(Dropped from the work doc: the PBIP one-owner rule — irrelevant unless you're
doing Power BI here.)*

---

## 5. Compliance boundary — INVERTED (the section that matters most)

The public-repo risk is anything **work-derived** leaking out under your name:
proprietary SQL, JobBOSS2 schemas, sample rows with real part numbers, a
screenshot with a drawing number, ITAR-controlled technical data.

- [ ] **Never commit work artifacts.** Full stop.
- [ ] **Rebuild work-taught techniques on synthetic/public data.** Learned a
      de-dup or scrap-closing pattern? Reproduce it against a dataset you
      *generate*, not one you exported.
- [ ] **Scrub sample data** to invented values before it goes near a commit.
- [ ] Push protection on, so a stray credential is blocked at push time.
- [ ] No internal screenshots. If you must show output, regenerate it from
      synthetic data.

This protects your job and your standing far more than any branch rule.

---

## 6. Optional brand signal (do if useful, not as ceremony)

- [ ] **Issues as a public roadmap / learning log** — shows you work
      ticket-driven. Skip rigid YAML intake forms; filing forms to yourself is
      theater.
- [ ] **A public project board** — "I run my learning like an engineer." Low
      priority.

---

## 7. Verify once (simplified — no second human to test against)

- [ ] Open a branch, push a trivial change, open a PR into `main`.
- [ ] Confirm you **cannot push straight to `main`** (ruleset blocks it).
- [ ] Confirm **CI runs** and the PR **can't merge until `test` is green**.
- [ ] Squash-merge → branch auto-deletes, history stays one-commit-per-ticket.

---

## Why this is the DE signal (the through-line for LinkedIn)

The work doc's arc was *author → automate → govern*. Personal public repos are
the **better stage for the "automate" step** than a locked-down ITAR org nobody
can see:

- A **green CI badge** on a public repo running lint + tests on every PR is a
  visible, verifiable "I enforce a reviewed, tested change process" —
  the baseline SWE rigor most analytics people never build.
- It transfers directly to dbt models, pipeline code, and IaC.
- The narrative line: *"I don't just write scripts — I ship them through a
  reviewed, tested pipeline, even solo."* That's a resume bullet and a post.
