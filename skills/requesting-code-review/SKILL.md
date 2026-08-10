---
name: requesting-code-review
description: Use when completing tasks, implementing major features, or before merging to verify work meets requirements
---

# Requesting Code Review

Run review along two independent axes: **Standards** and **Spec**. Keep their findings separate so documented-rule violations do not mask requirement gaps, and requirement gaps do not get hidden behind style or quality feedback.

## When to request review

Mandatory:

- After each task in subagent-driven development.
- After completing a major feature.
- Before merging to the mainline branch.

Useful:

- When stuck and needing a fresh read.
- Before risky refactoring.
- After a complex bug fix.

## Inputs

- Exact diff range or review package path.
- Repository standards files: instructions, style guides, test rules, architecture docs, PR templates, or local agent instructions.
- Originating issue, plan, spec, ticket, or requirements. If none exists, record `No spec available`.
- Verification evidence already run by the implementer.

## Dispatch

Use one `task` batch so independent reviewers run in parallel.

### Standards reviewer

Read-only. Give it:

- the exact diff range or review package;
- repository standards files;
- verification evidence.

Ask for:

- violations of documented rules, with file/line evidence;
- judgment-call smells clearly labelled as judgment calls;
- severity inside the Standards axis only.

### Spec reviewer

Skip this reviewer only when no issue, plan, spec, ticket, or requirement exists. In that case, write `No spec available` under the Spec heading.

When a spec exists, give it:

- the same exact diff range or review package;
- the originating issue/plan/spec/ticket/requirements;
- verification evidence.

Ask for:

- missing requirements;
- incorrect behavior;
- scope creep;
- source quotations from the spec for every finding;
- severity inside the Spec axis only.

## Aggregate

Do not rerank findings across axes. Preserve the two reports under these headings:

```markdown
## Standards

[Standards reviewer findings]

Finding count: N
Worst finding: [severity + one-line summary, or None]

## Spec

[Spec reviewer findings, or No spec available]

Finding count: N
Worst finding: [severity + one-line summary, or None]
```

If either reviewer reports Critical or Important findings, fix them before proceeding. If a reviewer cannot verify a requirement from the diff, resolve that item yourself using the broader plan/context before marking review complete.

## Act on feedback

- Fix Critical issues immediately.
- Fix Important issues before continuing.
- Track Minor findings explicitly if not fixed now.
- Push back only with code, tests, or source quotations proving the finding is wrong.
- If a finding conflicts with the plan text, ask which governs before changing code.

## Common rationalizations

| Excuse | Reality |
|--------|---------|
| "I'll just review the diff myself instead of dispatching reviewers" | You are the coordinator. Reviewing the diff inline burns the context window you need to keep driving the work. Dispatch the reviewers: the diff and the evaluation live in their context, and only the findings come back to you. |
| "The reviewers need my whole session history to understand the change" | Hand them precisely crafted context, never your session history. That keeps each reviewer on the work product, not your thought process. |
| "Standards and Spec overlap, so one reviewer is enough" | Collapsing the axes lets style noise bury a missing requirement. Run both; keep the reports separate. |

See [code-reviewer.md](code-reviewer.md) for reviewer prompt templates.
