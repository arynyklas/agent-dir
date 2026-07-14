---
name: pr-writer
description: Use when the user asks to create, open, submit, or update a pull request.
---

# PR Writer

Create or update GitHub pull requests using repository conventions and the Oh My Pi `github` tool surface.

**Dependency:** If requested PR contents include uncommitted changes, invoke `commit` first. Do not create a PR from an uncommitted working tree.

## Inputs to collect

- Current branch and intended base branch.
- Commit range and diff that will enter the PR.
- Existing issue, ticket, spec, or plan references supplied by context.
- Repository PR template, if one exists.
- Verification already run for the change.
- Whether the user explicitly wants ready-for-review. If not, create a draft.

## Analyze before writing

1. Confirm the branch is not the base branch.
2. Confirm all intended changes are committed.
3. Review the commits and diff for scope.
4. Identify the PR title from the primary commit or repository convention.
5. Read repository PR templates such as `.github/pull_request_template.md`, `.github/PULL_REQUEST_TEMPLATE.md`, or files under `.github/PULL_REQUEST_TEMPLATE/` when present.

## Body rules

### If a repository template exists

Follow it. Keep required headings, checklists, and sections, including any `Test Plan` section. Fill only what the change can support with evidence. Do not delete a template section just because a global workflow prefers a different shape.

### If no template exists

Use this order exactly, omitting empty sections:

```markdown
## What changed

## Why

## Verification

## Risks / reviewer notes

## Issue links
```

Guidance:

- `What changed`: concrete behavior or files affected, not a copied diff.
- `Why`: user problem, bug, requirement, or maintenance reason.
- `Verification`: exact commands, smoke checks, browser/manual scenarios, or `Not run` with a reason when no verification was applicable.
- `Risks / reviewer notes`: migration risk, compatibility notes, follow-up reviewer attention, or none if empty.
- `Issue links`: `Fixes #123`, `Refs #123`, or external tracker keys only when context supplies them.

## Create a PR

Use the harness GitHub operation, not shell heredocs:

```text
github op: pr_create
```

Set:

- `title`: repository-conventional PR title;
- `body`: the completed template/body;
- `base`: detected or requested base branch;
- `head`: current branch when needed;
- `draft`: `true` unless the user asks for ready-for-review.

Do not push, merge, or mutate trackers unless the user requested that separate action.

## Update an existing PR

The `github` wrapper has PR creation but no edit operation. For existing PR title/body updates only, use one focused `gh api -X PATCH repos/{owner}/{repo}/pulls/{number}` call. Do not combine unrelated label, reviewer, branch, or issue mutations into that PATCH.

## Review before submitting

Before creating or updating:

- Body follows repository template or fallback section order.
- Verification evidence is specific and current.
- Draft/ready state matches the user request.
- Issue links are supplied by context, not invented.
- No AI attribution or generated-by markers are added unless repository/user policy requires them.
