# VibeFlow-Cursor — Docs-first vibe coding workflow for Cursor ⚡

Welcome! This repo helps you **code with great vibes**: clear docs, crisp specs, traceable work, and consistent releases. It’s a **drop-in, docs-as-code workflow** (with Cursor rules + slash commands) that keeps projects fast and tidy.

> If this helped you, **⭐️ star** the repo and **🍴 fork** it to make it your own!

---

## What you get (functions & benefits)

* **Segmented development process** — A precise pipeline: **PRD → SPEC → FEATURE → Code/Tests → OP-NOTE → Release** with human review at every stage.
* **Docs version control** — Date-stamped files and “supersedes” links for auditable history.
* **Lean, unambiguous TECH SPECS** — Architecture = **one diagram** (frameworks + relationships) + **one component inventory**. No fluff.
* **Hard gates for risky changes** — Snapshot rules for **contracts, topology, framework roles, SLOs** (“tripwires”).
* **Traceability that sticks** — The same **feature ID** in filenames, branches, PR titles, and commits.
* **Slash commands for Cursor** — `/spec`, `/adr`, `/develop`, `/test`, `/eval`, `/debug`, etc. to standardize outputs.
* **Ops first-class** — OP-NOTEs (runbooks) required for production-impacting changes.
* **Name it once, everywhere** — Consistent naming for PRDs, SPECs, FEATUREs, ADRs, OP-NOTEs.
* **Docs-first→Code-second** — Code must follow (not invent) the contracts you documented.

---

## Quick start

1. **Fork** this repo (please do! 🙏) and **Star** it if you like the vibe.
2. Copy the rules into your project:

```
.your-repo/
  .cursor/
    rules/
      00-workflow.mdc
      01-prd.mdc
      02-tech_specs.mdc
      03-adrs.mdc
      04-features.mdc
      05-commands.mdc
      06-op-notes.mdc
      07-markdown.mdc
    docs/
      prds/      # PRDs: prd-YYYYMMDD.md
      specs/     # TECH SPECS: spec-YYYYMMDD-<spec>.md
      adrs/      # ADRs: adr-YYYYMMDD-<slug>.md
      features/  # FEATUREs: ft-<ID>-<slug>.md + schedule.md
      op-notes/  # OP-NOTEs: op-<ID>-<slug>.md or op-release-<semver>.md
```

3. Commit with a message like:

```
chore(rules): add docs-first workflow + commands
```

---

## Naming cheatsheet

* **PRD**: `docs/prds/prd-YYYYMMDD.md`
* **SPEC**: `docs/specs/spec-YYYYMMDD-<spec>.md` (e.g., `system`, `api`, `frontend`, `llm`)
* **ADR**: `docs/adrs/adr-YYYYMMDD-<slug>.md`
* **FEATURE**: `docs/features/ft-<ID>-<slug>.md` → e.g., `ft-123-hover-badge.md`
* **Schedule**: `docs/features/schedule.md`
* **OP-NOTE**: `docs/op-notes/op-<ID>-<slug>.md` or `op-release-<semver>.md`

> Use the same `<ID>` in branch, PR title, and commit messages.

---

## The workflow (at a glance)

1. **PRD** — Problem, users, scope, success metrics.
2. **SPEC** — `spec-YYYYMMDD-<spec>.md` with:

   * **Architecture diagram** (frameworks + relationships): Django/DRF, React, Redis, Celery, Postgres, Nginx/CDN, LLM, etc.
   * **Component inventory table** (framework/runtime, purpose, interfaces in/out, deps, scale/HA, owner).
3. **ADR** — Non-trivial choices (deps, storage, auth, SLO moves).
4. **FEATURE** — `ft-<ID>-<slug>.md` (acceptance criteria, design diffs, test/eval plan, rollout, risks).
5. **Code & Tests** — Only after docs; use `/develop`, `/test`, `/eval`.
6. **OP-NOTE** — Deployment/runbook.
7. **Release** — Tag, verify, close the loop.

**Docs-first guarantee:** The rules enforce generating/updating docs **before** code. Any change to **contracts / topology / framework roles / SLOs** requires a **new dated SPEC snapshot**.

---

## Example: Add the “Hover Badge” feature (ID `123`)

### 1) Create a PRD

`docs/prds/prd-20250820.md`

```md
# PRD — Article Credibility v1
Summary: Show G/Y/R badge with rationale to improve trust and reduce bounce.
Success: p95 ≤ 400ms; +2pp CTR on “Explain”.
```

### 2) Write/update SPECs

`docs/specs/spec-20250822-api.md`

```md
# Tech Spec — api
## Architecture
# One diagram (frameworks + relationships)
# One component inventory table (framework/runtime, interfaces, deps, scale/HA, owner)
```

If you changed API schema or topology, this is a **new dated snapshot**.

### 3) Record decisions

`docs/adrs/adr-20250822-cache-policy.md` (why Redis TTL=24h, etc.)

### 4) Plan the feature

`docs/features/ft-123-hover-badge.md`

```md
# Feature — 123 hover-badge
Acceptance:
- [ ] Badge renders G/Y/R within 200ms on cache hit
- [ ] Tooltip shows ≤3 rationales
Test & Eval:
- Unit/integration tests; goldens F1 ≥ 0.72
```

Update `docs/features/schedule.md` with ID, title, status.

### 5) Implement with Commands

Use the **Commands rule** during Stage E/F.

```
/develop 123 hover-badge
Context:
- Wire React to /api/v1/score; add Redis cache touchpoints
Artifacts:
- SPEC: docs/specs/spec-20250822-api.md
- FEATURE: docs/features/ft-123-hover-badge.md
```

Cursor should output:

* Plan, **code snippets/diffs**, **tests**, and **commit messages**:

```
feat(badge): add /v1/score integration (#123)
test(api): contract test for ScoreV1 (#123)
```

### 6) Validate & gate

```
/test 123
/eval 123
```

Add/refresh unit & integration tests; run AI evals on goldens with thresholds.

### 7) Prepare release

`docs/op-notes/op-123-hover-badge.md`
Deploy steps, monitoring dashboards, rollback, smoke checks.

---

## Why this approach works

* **Speed without chaos** — Small, focused docs that gate the right changes.
* **Auditability** — Date-stamped snapshots + ADRs explain “why”.
* **Consistency** — One naming scheme, one pipeline, fewer surprises.
* **Team clarity** — Everyone can find the current PRD/SPEC/FEATURE quickly.

---

## FAQ

**Q: Do I need multiple SPEC files?**
A: Split by scope when lifecycles differ: `system`, `api`, `frontend`, `llm`. All live in `docs/specs/` (no subdirs), date-stamped.

**Q: When do I snapshot a new SPEC?**
A: On changes to **contracts, topology, framework roles, or SLOs**. Typos/clarifications → just update the Changelog.

**Q: Semver or dates?**
A: **Dates for doc filenames**; semver **inside** specs for interfaces/models (e.g., “API v1.3”).

---

## Contribute

Issues and PRs welcome! If this saved you time or made your project feel calmer, **please ⭐ star** and **🍴 fork**. Share improvements to rules, examples, or commands.

---

## License

Use what fits your project (MIT/Apache-2.0). Add a `LICENSE` file accordingly.
