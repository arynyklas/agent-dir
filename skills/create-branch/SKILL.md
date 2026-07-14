---
name: create-branch
description: Use when the user asks to create, start, switch to, or name a Git branch, or when a commit workflow must leave the default branch.
argument-hint: '[optional description of the work]'
---

# Create Branch

Create a Git branch named `<type>/<short-description>` using the repository's convention when one is documented, otherwise the neutral convention below.

## Branch contract

`<type>` is one of:

| Type | Use when |
|---|---|
| `feat` | New user-facing functionality |
| `fix` | Broken behavior now works |
| `ref` | Same behavior, different structure |
| `chore` | Maintenance for existing tooling, dependencies, or config |
| `perf` | Same behavior, faster |
| `style` | Formatting or visual-only changes |
| `docs` | Documentation only |
| `test` | Tests only |
| `ci` | CI/CD config |
| `build` | Build system |
| `meta` | Repository metadata |
| `license` | License changes |

`<short-description>` rules:

- Lowercase ASCII kebab-case.
- 3-6 words.
- Describe the change, not file names.
- Use only ASCII letters, digits, and hyphens.

Examples:

| Work | Branch |
|---|---|
| Dropdown menu not closing on outside click | `fix/dropdown-not-closing-outside` |
| Add search to conversation page | `feat/add-conversation-search` |
| Simplify drawer components | `ref/simplify-drawer-components` |
| Update test fixtures | `chore/update-test-fixtures` |
| Add license notice | `license/add-mit-notice` |

## Workflow

1. **Read repository instructions first.** If local instructions define a branch convention, use it instead of the neutral type list above.
2. **Inspect current state without mutating it.** Determine:
   - current branch;
   - default branch from the remote HEAD or local `main`/`master` fallback;
   - whether HEAD is detached;
   - whether the current branch is non-default;
   - whether uncommitted changes exist;
   - whether the proposed name already exists locally or remotely.
3. **Choose the description.** Use the user-provided argument when present. Otherwise infer only from the requested work. Inspecting uncommitted diffs is allowed for naming, but do not assume every dirty file belongs to the requested branch.
4. **Classify the type.** Prefer the repository convention. Otherwise use the table above; when unsure, use `feat` for new behavior, `fix` for broken behavior, `ref` for structural-only changes, and `chore` for maintenance of something existing.
5. **Propose before mutating.** Show the proposed branch name and base branch/commit. Create nothing until the user approves both.
6. **Ask only when the answer changes the result.** Ask if:
   - HEAD is detached and the branch could be based on either the current commit or the default branch;
   - the current branch is not the default branch and the branch could be based on either current or default;
   - uncommitted work must be preserved, moved, stashed, or left behind;
   - the proposed branch name conflicts locally or remotely.
7. **Create after approval.** Create the approved branch from the approved base, preserving unrelated uncommitted work according to the user's selected handling.

## Safety rules

- Do not create a branch from a detached HEAD without explicit approval of that commit as the base.
- Do not silently switch away from a non-default branch.
- Do not stash, reset, discard, or move uncommitted changes without approval.
- Do not overwrite an existing local or remote branch name.
- If a repository has no discoverable default branch, ask for the base instead of guessing.
