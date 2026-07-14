# Skill Design Principles

Use this reference during initial skill design and refactoring. It adapts useful upstream skill-design vocabulary without importing a duplicate skill.

## Predictability

A skill should make the next action obvious. Name the trigger, the first move, the stopping condition, and the output shape. If an agent can choose between two plausible workflows, add an observable condition that chooses for it.

## Context load vs cognitive load

- **Context load:** tokens/files the agent must read.
- **Cognitive load:** decisions the agent must hold in working memory.

Reduce both. Put the common path in `SKILL.md`; push heavy details to references. Do not force-load large files for rare branches.

## Information hierarchy

Order content by use:

1. trigger and core principle;
2. mandatory workflow;
3. output contract / completion criteria;
4. exceptions keyed to observable predicates;
5. examples and references.

Important rules belong before examples. Examples illustrate; they do not carry requirements.

## Context pointers and progressive disclosure

Reference artifacts by path or URL instead of duplicating them. Load supporting files only when their branch is reached. A pointer is good when the agent can decide whether it needs the detail.

## Completion criteria

State what “done” means. A skill without completion criteria invites premature completion. Criteria should be observable: file written, report shape, command result, review verdict, artifact URI, or user approval gate.

## Co-location

Keep rules beside the template, checklist, or branch that uses them. A reminder far from the action is sediment. If a rule applies only to one output section, put it inside that section.

## Leading words

Use headings and bullets that start with the decision/action word: `Ask when`, `Do not`, `Output`, `Stop if`, `Verify`. Agents scan leading words under pressure.

## Prune failure modes

Remove:

- **No-ops:** rules that cannot change behavior.
- **Sediment:** old context, anecdotes, and historical explanations.
- **Sprawl:** repeated guidance in multiple places.
- **Duplication:** copied workflows better referenced by skill name.
- **Premature completion:** phrases that let the agent stop at a scaffold, draft, or partial checklist.
- **Negative-only steering:** long `don't` lists without a positive output recipe. For shape problems, define the exact shape instead.

## Application checklist

Before deploying a new or refactored skill:

- [ ] The trigger is specific and does not summarize the workflow.
- [ ] The first action is explicit.
- [ ] The output shape is defined when shape matters.
- [ ] Completion criteria are observable.
- [ ] Heavy references are linked, not inlined.
- [ ] Exceptions are keyed to observable conditions.
- [ ] No no-ops, sediment, sprawl, or duplicate workflows remain.
