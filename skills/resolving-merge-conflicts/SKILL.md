---
name: resolving-merge-conflicts
description: Use when you need to resolve an in-progress git merge/rebase conflict.
---

# Resolving Merge Conflicts

Resolve an in-progress merge or rebase without losing either side's intent.

## Workflow

1. **See current state.** Enumerate unresolved conflict blocks with `read <path>:conflicts` for each conflicted file. Read surrounding code before editing.
2. **Find primary sources.** For each side, understand why the change exists: commit messages, PRs, issues, specs, comments, and nearby code. Do not rely on “ours” or “theirs” labels as intent.
3. **Resolve each hunk.** Preserve both intents when compatible. If intents conflict, choose the resolution matching the merge/rebase goal and record the trade-off.
4. **Ask before destructive choices.** If a resolution discards one side, rewrites behavior, or removes a migration/validation/API change, ask first.
5. **Never abort automatically.** Do not abandon the merge/rebase unless the user explicitly chooses that path.
6. **Stage narrowly.** Stage only files whose conflicts you resolved. Do not blanket-stage unrelated files.
7. **Run focused checks.** Discover and run repository-native checks that cover the resolved files or changed path.
8. **Finish safely.** Invoke `commit` before creating a merge commit or continuing a rebase so staged scope is checked. A rebase continuation may reuse the existing commit message after the `commit` workflow confirms staged scope; author a replacement message only if Git asks for one.

## Red flags

- “Just take ours/theirs.” Intent still needs tracing.
- A conflict block touches validation, security, migrations, public API, or data shape.
- Resolution would silently drop tests or docs from one side.
- More files are staged than the conflicts you resolved.
- The check you ran does not cover the resolved path.

## Output

Report:

- files resolved;
- intent from each side;
- chosen resolution and trade-off;
- checks run and results;
- whether `commit` was invoked before final merge/rebase continuation.
