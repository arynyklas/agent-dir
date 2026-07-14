---
name: commit
description: Use when the user asks to commit changes, save work in Git, or requests a commit message.
---

# Commit

Create one focused Git commit using the repository's documented convention when one exists. If the repository has no convention, use the neutral conventional-commit header:

```text
<type>(<scope>): <subject>
```

`(<scope>)` is optional. The subject is imperative, present tense, capitalized, no final period, and at most 70 characters. Keep message lines under 100 characters.

## Before staging

1. **Read repository instructions.** Local commit conventions, issue-link rules, and attribution policy override this global skill.
2. **Check the branch.** If the current branch is the default branch, invoke `create-branch` before committing unless the user explicitly authorized committing on the default branch. If branch creation is declined or does not leave the default branch, stop.
3. **Identify requested scope.** List the files that belong to the user's requested change. Treat unrelated dirty files as the user's work and leave them untouched.
4. **Inspect the diff.** Understand what changed before staging. Do not stage files merely because they are dirty.

## Stage safely

- Stage only files belonging to the requested change.
- Never use blanket staging when unrelated changes may exist.
- Leave unrelated modified, deleted, or untracked files exactly as they are.
- After staging, verify the staged diff. If it contains unrelated work, unstage it before committing.
- If the requested change itself is incomplete or unverified, stop instead of committing.

## Types

| Type | Purpose |
|---|---|
| `feat` | New feature |
| `fix` | Bug fix |
| `ref` | Refactoring with no behavior change |
| `perf` | Performance improvement |
| `docs` | Documentation only |
| `test` | Test additions or corrections |
| `build` | Build system or dependencies |
| `ci` | CI configuration |
| `chore` | Maintenance tasks |
| `style` | Formatting or visual-only changes |
| `meta` | Repository metadata |
| `license` | License changes |

Use repository-specific types if documented.

## Message body

Include a body when it adds information reviewers cannot get from the diff:

- what changed;
- why it changed;
- previous behavior when relevant;
- verification evidence when repository convention expects it.

Do not narrate implementation mechanics that are obvious from the diff.

## Footer references

Use issue references only when context supplies an issue, ticket, or tracker key. Do not invent one.

Preferred neutral GitHub references:

```text
Fixes #123
Refs #123
```

Use an external tracker key only when it is already established by the issue, branch, plan, repository instructions, or user request:

```text
Refs ABC-123
```

`Fixes` closes the issue when merged; `Refs` links without closing.

## AI attribution

Do not add AI attribution, co-author trailers, or generated-by markers unless repository or user policy explicitly requires them. Follow the repository's normal author attribution rules.

## Examples

```text
fix(api): Handle null user response

Deleted users could produce a null API response and crash callers.
Return a not-found error before accessing user fields.

Fixes #123
```

```text
ref: Extract shared validation helper

Move duplicate validation from three call sites into one helper. Behavior is unchanged.
```

```text
docs: Clarify local setup steps

Refs ABC-123
```

## Reverts

Use the standard revert shape unless repository instructions say otherwise:

```text
revert: feat(api): Add batch endpoint

This reverts commit abc123def456.

Reason: Caused a production performance regression.
```

## Completion criteria

Before committing, confirm:

- the branch is allowed for commits;
- only requested files are staged;
- the staged diff matches the intended change;
- verification required for the requested change has been run or intentionally documented as not applicable;
- the message follows repository convention or the neutral header format above.
