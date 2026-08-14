# Domain Reviewer Templates

Ten domains for whole-repo audit. Select on evidence from the Phase 1 map, dispatch the selected ones in a single `task` batch, and give each a bounded slice rather than the repository.

## Shared rules for every reviewer

Put these in every reviewer prompt:

- Read-only. Do not mutate files, index, branch, tracker, or CI state.
- Audit only the assigned slice. Read the whole slice; do not sample it.
- Do not run project-wide suites, formatters, or linters.
- Every finding needs `file:line` plus the wrong behaviour a user or caller would observe. A finding you cannot support is dropped, not softened.
- A reproduction beats a citation. Where a stub is cheap, write one and paste the output.
- Separate evidence from judgment. Label judgment as `Judgment call`, and unproven suspicion as `[INFERENCE]` with the check that would settle it.
- Report what you examined and found clean, not only what you found broken.
- Never infer intent from a name. Read the implementation.

## Prompt template

```markdown
You are the [DOMAIN] reviewer auditing an existing codebase.

## Repository map
[PHASE_1_MAP]

## Your slice
[EXPLICIT FILE OR MODULE LIST]

## Repository standards
[STANDARDS_FILES_OR_PATHS, or "none documented"]

## Your checklist
[CHECKLIST FROM THE DOMAIN SECTION BELOW]

## Task
Audit your slice against your checklist. Work the checklist item by item; every
item gets an answer, including the ones that come back clean.

## Output format

### Findings
#### Critical
- [file:line] Title
  - Evidence: [citation, or reproduction command plus observed output]
  - Observable: [what a user or caller actually gets]
  - Blast radius: [how far a wrong result travels before anyone notices]
  - Fix: [specific direction]

#### High / Medium / Low
[same shape]

#### Judgment calls
[same shape, explicitly labelled]

### Inferences
- [claim] - would be settled by [check]

### Coverage
Checklist items clean: [list]
Files in slice not read: [list, with reason - ideally none]

### Count
N findings

### Worst finding
[Severity + one-line summary, or None]
```

## correctness

Select always. This is the domain that catches code reporting success while being wrong, which survives review because every reading of the source looks reasonable and the happy path passes.

- **Success signal ignoring the outcome.** A failure path that prints and returns instead of exiting non-zero; an exception caught, logged, and swallowed; a helper returning `None` where the caller tests only truthiness. Ask what a `&&` chain, a CI step, or an agent loop reads afterwards.
- **Responses treated as data.** `.json()` with no status check; an error envelope parsed as an empty result set; an HTML login page parsing as zero records.
- **Pagination taken as a whole.** `results`, `items`, or `data` read from one response while the API returns a cursor, `next`, or `has_more`. The symptom is a short list that looks complete, so a lookup past the boundary reports "not found" for a record that exists.
- **Unescaped interpolation.** User or database text composed into HTML, SQL, shell, a regex, or a path. Corrupted output is the common case; injection is the tail.
- **Time, encoding, locale.** Naive datetimes compared against aware ones; bytes decoded with the platform default; a BOM-less file read as the local codepage; `,` versus `.` as decimal separator. These produce plausible wrong values rather than exceptions.
- **Partial writes.** A multi-step write with no transaction or rollback; read-modify-write with no locking; a file written in place rather than temp-and-rename.
- **Unreachable or contradictory branches.** Conditions that cannot fire, duplicated `if` arms, dead error handling.

## security

Select when the repo handles input, credentials, or network traffic.

- Secrets in source, in history, or as the default of an environment lookup. A working credential as a fallback means every unset environment authenticates as one shared identity.
- Verification switched off: `verify=False`, `rejectUnauthorized: false`, `curl -k`, `StrictHostKeyChecking=no`, muted TLS warnings. Usually a private CA missing from the shipped bundles; the fix points at the CA rather than disabling the check.
- Injection reachable from untrusted input: SQL, shell, path traversal, deserialization, SSRF.
- AuthN/AuthZ: missing checks, checks after the side effect, IDOR, tokens without expiry or audience.
- Input trust boundaries: validation on the client only, mass assignment, unbounded uploads.
- Logging that records credentials, tokens, or personal data.

## performance

Select when there is a data layer, a hot loop, or a user-facing latency path.

- N+1 queries and queries inside loops.
- Missing indexes for the predicates actually used.
- Unbounded reads: no pagination, no limit, whole table or directory into memory.
- Repeated work that is not memoized, and cache keys that miss the varying input.
- Algorithmic cost on input that grows; nested scans over collections.
- Sync I/O on an async path, and unnecessary serialization of independent work.

## architecture

Select when the repo has more than a handful of modules. Use `codebase-design` vocabulary: module, interface, depth, seam, adapter, leverage, locality, deletion test.

- Shallow modules: a large interface hiding little behaviour.
- Leaked abstractions forcing callers to know internals.
- Cyclic or upward dependencies across layers.
- Duplicated logic that should sit behind one seam.
- God modules, and dumping-ground `utils`.
- Missing seams at the boundaries that most need substitution: clock, network, filesystem, randomness.

## tests

Select always; a repo with no tests is itself the finding.

- **Assertions that cannot fail.** Expected values computed by the code under test, a stubbed-out subject, a mock asserting only that it was called, a snapshot regenerated on mismatch.
- Coverage concentrated on trivia while entry points and error paths go untested.
- Tests coupled to implementation detail, so a refactor breaks them and a behaviour change does not.
- Order dependence, shared mutable state, real network or real clock, hardcoded sleeps.
- Skipped, commented-out, or permanently failing tests.
- Whether the suite actually runs at the pinned dependency versions.

## data

Select when a schema, migration, or ORM is present.

- Migrations that are irreversible, unordered, or not idempotent.
- Constraints enforced only in application code: nullability, uniqueness, foreign keys.
- Type choices that lose data: money in floats, timestamps without zone, enums as free text.
- Transaction scope too wide, too narrow, or absent around multi-statement writes.
- No backup or restore path for anything stateful.

## api

Select when the repo exposes or consumes an interface others depend on.

- Breaking changes with no versioning story.
- Error semantics: wrong status codes, errors returned as 200, inconsistent error shapes.
- Contract drift between implementation, schema, and documentation.
- Missing idempotency on retryable writes; no timeout or retry budget on outbound calls.
- Pagination, filtering, and sorting that the client cannot use reliably.

## frontend

Select when there is UI code.

- Accessibility: keyboard traps, missing labels and roles, contrast, focus management.
- State handling: derived state stored, effects that loop, stale closures.
- Render cost: unkeyed lists, work in render, unnecessary re-render cascades.
- Unsanitized HTML injection sinks.
- No loading, empty, or error state for anything asynchronous.

## ops

Select when CI, containers, or deployment configuration exist.

- **Stale artefacts presented as fresh.** A cache keyed on something that did not change, a build output not rebuilt, a container tag reused, a lockfile out of step with its manifest, a package republished at an unchanged version. The tell is a success message with unchanged behaviour.
- CI that reports green without running the thing it claims to run, or that cannot fail.
- Secrets in workflow files, logs, or images.
- Unpinned dependencies and base images; no lockfile.
- **Environment-shaped commands.** A documented or embedded command that only runs on the author's machine: a missing interpreter, a hardcoded absolute path, a helper assumed on `PATH`, a subprocess that prompts with no console.
- **Tooling pointed at the wrong system.** A helper defaulting to a public service while the repo lives on a self-hosted one, so it silently answers about somewhere else.
- No health check, no rollback path, no observability on a deployed service.

## docs

Select always, and give it the map plus the README and any agent instructions.

- Setup instructions that do not work: run them.
- Documented commands, flags, or endpoints that no longer exist.
- Examples referencing moved or deleted files.
- Claims of features that are absent, or absence of features that exist.
- Stale version numbers and changelogs.
- Agent instruction files describing a structure the repo no longer has.
