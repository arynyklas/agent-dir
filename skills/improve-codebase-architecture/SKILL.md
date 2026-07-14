---
name: improve-codebase-architecture
description: Scan a codebase for deepening opportunities, present them as a visual HTML report, then grill through whichever one you pick.
disable-model-invocation: true
---

# Improve Codebase Architecture

Surface architectural friction and **deepening opportunities**: refactors that turn shallow modules into deeper modules. Use `codebase-design` vocabulary exactly: module, interface, depth, seam, adapter, leverage, locality, deletion test.

## Scope before scanning

Deepening pays off where future change is likely. Scope the audit before reading broadly:

1. If the user names a subsystem, module, pain point, or direction, use that scope.
2. Otherwise prefer repo-intel hotspots when available.
3. If no repo-intel artifact exists, use read-only Git history to identify recently changed areas.
4. If changes are scattered, widen the net gradually.

Read domain vocabulary and ADRs for the scoped area before proposing candidates.

## Explore

Use built-in `read`, `glob`, and `grep` for focused inspection. For broad codebase exploration, dispatch one `task` batch of read-only scout agents scoped to distinct subsystems. Ask scouts to report friction, shallow modules, seam leaks, testing pain, and deletion-test results; they must not edit.

Look for:

- concepts that require bouncing between many small modules;
- interfaces nearly as complex as their implementation;
- extracted pure functions that only exist for tests while real behavior leaks elsewhere;
- cross-seam coupling or duplicated adapters;
- code that is untested or hard to test through a public interface.

Apply the deletion test: if deleting a module would concentrate complexity instead of removing meaningful leverage, it is likely shallow.

## Present candidates first

Do not propose final interfaces yet. First present candidates in a self-contained HTML report.

Write the report to the OS temp directory and return the absolute path. Render or open it with the `browser` tool when supported. Do not write the report into the repository.

Each candidate card includes:

- files/modules involved;
- problem;
- solution direction, not final interface;
- benefits in terms of locality, leverage, and tests;
- before/after diagram;
- recommendation strength: `Strong`, `Worth exploring`, or `Speculative`;
- ADR conflict callout when real friction warrants revisiting an ADR.

Use [HTML-REPORT.md](HTML-REPORT.md) for scaffold and visual patterns.

End with a Top recommendation section and ask: “Which of these would you like to explore?”

## Grilling loop

After the user picks a candidate:

1. Use `grilling` to walk constraints, dependencies, seam placement, and what sits behind the interface.
2. Use `domain-modeling` when domain terms are added or sharpened.
3. Use `codebase-design` design-it-twice guidance when alternative interfaces should be explored.
4. Offer an ADR only when the user's rejection or decision would prevent future reviewers from re-suggesting the same candidate.

No branch, commit, tracker write, or implementation is part of this skill unless the user separately asks for it.
