---
name: prototype
description: Use when the user wants to sanity-check a state model or logic flow, or explore what a UI should look like with a throwaway prototype.
---

# Prototype

A prototype is **throwaway code that answers one design question**. The question decides the shape.

## Pick a branch

- Logic, state model, API shape, or data transitions → [LOGIC.md](LOGIC.md).
- Visual layout or interaction feel → [UI.md](UI.md).

If ambiguous and the user is reachable, ask. If not, choose the branch that matches nearby code and state the assumption at the top of the prototype.

## Rules

1. **Throwaway from day one.** Name and mark prototype files so a casual reader knows they are not production.
2. **Use isolation when touching a Git repo.** Invoke `using-git-worktrees` before creating prototype files in a repository unless the existing workspace is already isolated or the user explicitly wants in-place exploration.
3. **One command to run.** Use the project's existing task runner or print the exact command at the top of the prototype.
4. **No persistence by default.** Use memory, scratch files, or scratch databases with an obvious prototype name.
5. **Skip polish.** Prototype code does not require tests, broad error handling, or abstractions. It must be runnable, not production-ready.
6. **Surface state.** After every logic action or UI variant switch, show the relevant state so the user can react.
7. **No automatic commits.** Preserve or commit a prototype only when the user asks. If they ask, invoke `create-branch` and then `commit`.

## TDD boundary

Throwaway prototype code is exempt from normal test-driven-development. Production behavior copied from a validated prototype is not exempt: first use `brainstorming` for the intended behavior, then use `test-driven-development` to implement production code through the agreed public seam.

Do not promote prototype code directly. Treat it as a primary source for decisions, then rewrite production behavior under the normal workflow.

## Handoff

When the prototype answers its question, record:

- the question;
- the run command or URL;
- the observed answer/decision;
- whether the prototype should be deleted, kept as a local artifact, or preserved by branch/commit at the user's request.
