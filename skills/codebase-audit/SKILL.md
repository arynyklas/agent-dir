---
name: codebase-audit
description: Use when auditing a whole codebase rather than a diff - "audit the codebase", "review the project", "find the problems in this repo", inheriting unfamiliar code, pre-release quality sweep, or assessing a repo before committing to work on it.
---

# Codebase Audit

Audit an entire repository and return a ranked, evidence-backed list of what is wrong with it.

`requesting-code-review` reviews **a diff** against standards and spec. This skill reviews **a repository** with no diff and often no spec: the questions are what exists, what is dangerous, and what to do first. Use that skill for changes under review; use this one for inherited, unfamiliar, or pre-release code.

## The two ways an audit fails

Both are more likely than missing a bug, and both are why this skill front-loads mapping and evidence:

- **Findings nobody trusts.** Plausible-sounding entries with no proof. Reviewers of an unfamiliar repo guess at intent and report the guess. One wrong finding spends the reader's trust and teaches them to skim the rest.
- **Silent gaps.** A directory nobody opened reads exactly like a directory that passed. An audit that does not state its own coverage is unfalsifiable.

## Inputs

- Repository root, and the revision audited (`git rev-parse --short HEAD`).
- Standards files: agent instructions, style guides, architecture docs, test rules.
- Any spec, roadmap, or issue tracker, if one exists. Record `No spec available` when not.
- How many findings the requester intends to act on now. An audit that returns 60 items and fixes none was theatre.

## Phase 1: Map before opinions

Inventory first. Reviewers dispatched without a map re-derive the same shallow picture ten times in parallel and each report the same surface findings.

Establish, and write down:

- **Entry points** — what actually runs: CLIs, servers, handlers, jobs, migrations, scheduled tasks. Read `package.json` scripts, `pyproject.toml`, `Makefile`, CI workflows, `bin/`.
- **Modules** — the real units and their boundaries, plus line counts so you know where the mass is.
- **Dependency posture** — direct dependencies, lockfile presence and freshness, anything vendored or pinned to a commit.
- **Test and CI reality** — what suites exist, whether they run, what CI actually enforces. Never infer from the presence of a test directory.
- **Hot spots** — `git log` churn per file, and files changed most alongside bug fixes. High churn with no tests is where you point reviewers first.
- **Age and provenance** — first and last commit, number of authors, whether it is maintained or abandoned.

Done when you can name every entry point, say which modules carry the mass, and state what CI enforces. If `wayfinder` already produced a map, start from it.

## Phase 2: Select reviewers on evidence

Pick domains the map justifies. A database reviewer on a repo with no data layer returns filler, and filler is indistinguishable from a clean result.

`reviewers.md` holds the ten domain templates and their checklists. Select from the map: no data layer, no data reviewer. Record which domains you skipped and why — that record is half your coverage statement.

Done when every selected domain traces to something in the map, and every unselected domain has a one-line reason.

## Phase 3: Dispatch bounded slices in parallel

One `task` batch, read-only reviewers, per `dispatching-parallel-agents`. Two rules specific to whole-repo work:

- **Bound every slice.** A reviewer told to audit the repo skims it. Give each an explicit file or module list from the map, sized to be read properly. Large repos need the same domain dispatched across several slices rather than one reviewer with an impossible brief.
- **Hand over the map, not your history.** Reviewers get the Phase 1 map, their slice, the standards files, and their template. Never your session context.

Done when every selected domain has full coverage of its slices, and no reviewer was asked to read more than it could.

## Phase 4: Gate on evidence

Findings arrive over-confident. Every surviving finding carries one of:

- a **citation** — `file:line`, plus the specific wrong behaviour a user or caller would observe;
- a **reproduction** — the command, input, or stub that makes it observable, with the output seen.

Discard what cannot be supported; do not soften it into hedged prose. Keep a genuine suspicion as `[INFERENCE]` with the check that would settle it. Reconcile duplicates across reviewers into one finding, and treat the same defect found by three domains as a signal about severity.

Done when every finding is provable, deduplicated, and stripped of speculation.

## Phase 5: Rank by blast radius, then severity

Severity alone puts a scary-looking flaw in a hand-run script above a quiet one in a nightly job. Rank by how far a wrong result travels before anyone notices, then by severity within that.

- **Critical** — data loss, security breach, silent wrong output in production. Fix before anything else ships.
- **High** — likely bug or regression on a live path.
- **Medium** — maintainability or correctness risk off the hot path.
- **Low** — style, nit, cleanup.

## Phase 6: Report, route, and stop

```markdown
# Codebase audit: [repo] @ [revision]

## Verdict
[2-4 sentences: overall condition, and the single thing to do first.]

## Coverage
Domains audited: [list]
Domains skipped: [domain - reason]
Not reached: [paths, and why]

## Findings
### Critical
- [file:line] Title
  - Evidence: [citation or reproduction output]
  - Observable: [what a user or caller gets]
  - Blast radius: [how far it travels before anyone notices]
  - Fix: [specific direction]

### High / Medium / Low
[same shape]

## Inferences
- [claim] - would be settled by [check]
```

Route by cost, and never invent tracker commands: Critical and High get fixed now; Medium and Low go to `to-tickets`, which knows where this project's tracker is. Fix nothing during the audit itself — mixing findings and edits leaves neither reviewable.

Then stop. Do not chain into a refactor. Apply fixes only when asked, and when you do, use `verification-before-completion` on each.

## Common rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll skip the map and dispatch reviewers straight at the repo" | Unmapped reviewers all rediscover the same surface layer and none reaches the hot spots. The map is what makes ten reviewers cover ten different things. |
| "Spawn all ten domains, extras cost nothing" | An unjustified domain returns filler, and filler is indistinguishable from a clean result. It inflates the report and hides the real findings. |
| "The reviewer said it's a vulnerability, that's good enough" | Reviewers of unfamiliar code guess at intent and report the guess. Ungated findings are how audits lose their reader. |
| "I'll note the areas I didn't get to at the end if there's room" | Coverage is not a footnote. An unexamined directory reads exactly like a clean one, which makes the whole report unfalsifiable. |
| "60 findings shows how thorough I was" | A report nobody acts on is theatre. Rank it, cap what gets fixed now, and file the rest. |
| "I found the bug, I should fix it while I'm here" | Then the audit and the change review each other, and neither is reviewable. Report first, fix on request. |
