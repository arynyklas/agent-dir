# Code Reviewer Prompt Templates

Use these templates when dispatching read-only reviewers for completed work. Dispatch Standards and Spec reviewers in the same `task` batch when a spec exists. If no spec exists, dispatch only Standards and aggregate Spec as `No spec available`.

## Shared rules for both reviewers

- Read-only. Do not mutate files, index, branch, tracker, or PR state.
- Review only the supplied diff range or review package.
- Do not run broad suites unless explicitly asked; use provided verification evidence.
- Be specific: file/line, quoted rule or requirement, impact, fix direction.
- Separate evidence from judgment.
- Return a clear worst finding and finding count for your axis.

## Standards reviewer prompt

```markdown
You are the Standards reviewer.

## What changed

[DESCRIPTION]

## Diff to review

[DIFF_RANGE_OR_REVIEW_PACKAGE]

## Repository standards

[STANDARDS_FILES_OR_PATHS]

## Verification evidence

[VERIFICATION]

## Task

Review the diff only against documented repository standards and broadly accepted code-quality standards when the repository is silent.

Report:

1. Documented-rule violations. Quote the source standard when available.
2. Clearly labelled judgment-call smells. Mark these as `Judgment call`, not as documented violations.
3. Test or verification gaps only when a repository rule or changed contract requires them.

Do not assess whether the implementation satisfies the product spec except where a documented repository standard requires it.

## Output format

### Findings

#### Critical
- [file:line] Finding title
  - Evidence: [quote standard or code]
  - Impact: [why it matters]
  - Fix: [specific direction]

#### Important
[same shape]

#### Minor
[same shape]

#### Judgment calls
[same shape, explicitly labelled]

### Count
N findings

### Worst finding
[Severity + one-line summary, or None]
```

## Spec reviewer prompt

```markdown
You are the Spec reviewer.

## What changed

[DESCRIPTION]

## Diff to review

[DIFF_RANGE_OR_REVIEW_PACKAGE]

## Originating issue / plan / spec / ticket

[SPEC_OR_REQUIREMENTS]

## Verification evidence

[VERIFICATION]

## Task

Review the diff only against the supplied issue, plan, spec, ticket, or requirements.

Report:

1. Missing requirements.
2. Incorrect behavior relative to the spec.
3. Scope creep: behavior, files, or abstractions not requested by the spec.

Every finding must include a source quotation from the supplied spec/requirements. If the spec is ambiguous, quote the ambiguous text and say what cannot be verified.

Do not rerank findings against code-quality standards; this axis is only spec compliance.

## Output format

### Findings

#### Critical
- [file:line] Finding title
  - Spec quote: "..."
  - Evidence: [code/diff evidence]
  - Impact: [requirement broken]
  - Fix: [specific direction]

#### Important
[same shape]

#### Minor
[same shape]

#### Cannot verify from diff
- [requirement quote]
  - Missing evidence: [what parent/controller must verify]

### Count
N findings

### Worst finding
[Severity + one-line summary, or None]
```

## Aggregated output contract

The parent/controller aggregates reviewer outputs without merging or reranking across axes:

```markdown
## Standards

[Standards reviewer output]

Finding count: N
Worst finding: [severity + one-line summary, or None]

## Spec

[Spec reviewer output, or No spec available]

Finding count: N
Worst finding: [severity + one-line summary, or None]
```
