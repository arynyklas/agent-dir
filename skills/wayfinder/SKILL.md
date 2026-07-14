---
name: wayfinder
description: Plan a huge chunk of work — more than one agent session can hold — as a shared map of decision tickets on your issue tracker, and resolve them one at a time until the way to the destination is clear.
disable-model-invocation: true
---

# Wayfinder

Use Wayfinder when an idea is too large for one session and the route to the **destination** is hidden by **fog**. The deliverable is a map of **decision tickets**, not implementation.

## Vocabulary

- **Destination:** what the map is trying to make clear: a spec, decision, migration route, or handoff point.
- **Map:** the index artifact. It lists decisions made and points at tickets; it does not duplicate ticket detail.
- **Decision ticket:** one question sized for one fresh context session. Its resolution is a decision or fact that clears fog.
- **Frontier:** open, unblocked, unclaimed tickets that can be resolved now.
- **Fog:** in-scope uncertainty not sharp enough to ticket yet.
- **Out of scope:** known work beyond the destination.

Wayfinder is planning by default. Resolve decisions; do not implement the destination. The only ticket type that does work is a task that unblocks a decision.

## Storage

Use the repository's documented tracker when configured.

When no tracker is configured, use local files:

- map: `.scratch/<feature-slug>/wayfinder/map.md`
- tickets: `.scratch/<feature-slug>/wayfinder/issues/<NN>-<slug>.md`

The map remains an index. Each decision lives in exactly one ticket and is linked from the map after resolution.

## Map body

```markdown
## Destination

<what reaching the end looks like>

## Notes

<domain, skills to consult, standing preferences>

## Decisions so far

- `<closed ticket title>` — `<ticket path or URL>` — <one-line gist>

## Not yet specified

<in-scope fog not sharp enough to ticket>

## Out of scope

<ruled-out work beyond the destination>
```

## Ticket body

```markdown
## Question

<the decision or investigation this ticket resolves>

## Type

research | prototype | grilling | task

## Blocked by

<ticket titles or None>
```

## Ticket types

- **Research:** AFK reading of primary sources, docs, APIs, local resources, or code. Use `web_search` with primary-source reads, or read-only scout subagents for parallel local/external research.
- **Prototype:** HITL concrete artifact to react to. Use `prototype` when behavior or UI feel is the key unknown.
- **Grilling:** HITL conversation. Use `grilling` and `domain-modeling` one question at a time.
- **Task:** Work that must happen before a decision can be made. It exists only to unblock a decision, not to deliver the destination.

## Chart the map

1. **Name the destination.** Use `grilling` and `domain-modeling` to pin down what the map is finding its way to.
2. **Map the frontier breadth-first.** Surface the first sharp decisions and visible fog. If there is no fog and the path fits one session, stop and ask whether the user wants a normal spec/plan instead.
3. **Create the map.** Fill Destination and Notes, sketch fog into Not yet specified, leave Decisions so far empty.
4. **Create tickets you can specify now.** Create-then-wire blocking edges so the frontier is visible.
5. **Run research in parallel when useful.** Research tickets may be resolved by parallel read-only scout agents or `web_search` plus primary-source reads. Save findings as ticket resolutions or linked artifacts.
6. Stop. Charting is one session's work; it does not hand-resolve implementation.

## Work through a map

User invokes with a map path/URL/number. A ticket is optional.

1. Load the map, not every ticket body.
2. Choose one ticket: the named ticket, otherwise the first frontier ticket.
3. Claim it before work when the tracker supports assignment or a local status field.
4. Resolve it using only the zoom needed: ticket body, linked decisions, relevant closed tickets, and skills named in Notes.
5. Record the resolution in the ticket. Close or mark it resolved. Append only a one-line pointer to Decisions so far on the map.
6. Graduate newly clear fog into new tickets, create-then-wire blockers, and remove the graduated fog from Not yet specified.
7. If a ticket sits beyond the destination, close/mark it out of scope and add one line to Out of scope. Do not list it as a decision.

Never resolve more than one non-research ticket per session. Research tickets may run in parallel when independent.

## Persistence

Do not create branches or commits unless the user asks for persistence. When persistence is requested, use `create-branch` and `commit`.
