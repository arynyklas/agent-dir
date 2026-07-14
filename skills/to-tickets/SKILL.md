---
name: to-tickets
description: Break a plan, spec, or the current conversation into a set of tracer-bullet tickets, each declaring its blocking edges, published to the configured tracker — edges as text in one file per ticket locally, or native blocking links on a real tracker.
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, issue, or settled conversation into **tracer-bullet tickets**: vertical slices that each deliver a narrow end-to-end behavior and declare the tickets that block them.

## Context

Work from existing context. If the user passes a spec path, issue number, PR, or URL, read its full body and comments before drafting tickets. Use project domain vocabulary and respect relevant ADRs.

## Ticket shape

A good ticket:

- cuts through every required layer for one behavior: data, API, UI, tests, docs, or operations as applicable;
- is demoable or verifiable on its own;
- fits in one fresh agent context;
- names its blockers explicitly;
- describes user-observable behavior, not a layer-by-layer task list.

Reject horizontal breakdowns like “DB ticket, API ticket, UI ticket, tests ticket” unless the work is a wide refactor that cannot land green as vertical slices.

## Wide refactors

A **wide refactor** is one mechanical change whose blast radius crosses many call sites, so no vertical slice can safely land alone. Use expand-contract:

1. **Expand:** add the new form beside the old without breaking callers.
2. **Migrate:** move call sites in batches sized by blast radius, each blocked by Expand.
3. **Contract:** delete the old form after every migrate batch completes.

If batches cannot stay green independently, keep the sequence but use a shared integration branch and block a final integrate-and-verify ticket on all batches.

## Drafting process

1. Gather context from the referenced plan/spec/issue/conversation.
2. Inspect the codebase only as needed to understand existing seams and likely blockers.
3. Draft vertical tracer bullets with blocking edges.
4. Present the proposed breakdown as a numbered list with:
   - **Title**
   - **Blocked by**
   - **What it delivers**
5. Ask the user to approve granularity and blocker edges. Iterate until approved.

Do not publish tickets before approval.

## Publication rule

Use the repository's documented tracker when configured.

If no tracker is configured, write one ticket per file under `.scratch/<feature-slug>/issues/` only after user approval and outside plan mode. Number files in dependency order, blockers first: `01-<slug>.md`, `02-<slug>.md`, etc.

Use tracker-native blocking or sub-issue links when available. Otherwise list blockers in the ticket body.

## Local ticket template

```markdown
# <NN> — <Ticket title>

**What to build:** <end-to-end behavior this ticket makes work, from the user's perspective>

**Blocked by:** <numbers/titles of blockers, or "None — can start immediately">

**Status:** ready-for-agent

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
```

## Tracker ticket template

```markdown
## Parent

<parent issue/spec link, if one exists>

## What to build

<end-to-end behavior this ticket makes work, from the user's perspective>

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Blocked by

- <blocking ticket reference, or "None — can start immediately">
```

Avoid specific file paths or code snippets because they stale quickly. Exception: if a prototype produced a compact snippet that encodes a decision more precisely than prose can, inline only the decision-rich part and state that it came from a prototype.

## Execution handoff

After publication, do not execute tickets inside this skill. Use `subagent-driven-development` for independent tickets in one session, or `executing-plans` for sequential/checkpointed execution.
