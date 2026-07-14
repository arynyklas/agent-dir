---
name: to-spec
description: Turn the current conversation into a spec and publish it to the project issue tracker — no interview, just synthesis of what you've already discussed.
disable-model-invocation: true
---

# To Spec

Turn the settled conversation and codebase context into a spec. Do **not** interview the user during synthesis. If required information is missing, mark it in `Further Notes` instead of reopening discovery.

## Destination rule

Choose the destination deterministically:

1. Use the repository's documented spec or tracker convention when one exists.
2. If the user explicitly requests tracker publication, publish through that tracker only after its required approval or mutation gate.
3. Otherwise write the spec to `local://<slug>-spec.md`.

Before writing to the working tree or a tracker, use the harness plan approval gate. Local session artifacts under `local://` do not mutate the repository.

## Process

1. **Synthesize existing context.** Use only the conversation, referenced artifacts, issue/spec links, and focused codebase inspection needed to understand current state. Do not ask new interview questions.
2. **Name existing seams.** Use `codebase-design` terms: module, interface, seam, adapter, depth, leverage, locality. Prefer existing seams over new seams; propose new seams only when the current code has no suitable public interface. For domain terms, use `domain-modeling` vocabulary and existing project language.
3. **Write the spec.** Use the exact section names below, in order.
4. **Seek approval before repository or tracker mutation.** If the destination is `local://`, write the artifact directly and report the URI.

## Spec template

## Problem Statement

The problem the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A numbered list of user stories in this format:

1. As an <actor>, I want a <feature>, so that <benefit>

Cover the observable behavior without inventing requirements absent from the conversation.

## Implementation Decisions

Implementation decisions already established. This can include:

- modules to build or modify;
- interfaces and seams that will be used;
- adapters needed across seams;
- technical clarifications from the developer;
- architecture, schema, API, or interaction decisions.

Avoid specific file paths or code snippets because they stale quickly. Exception: if a prototype produced a compact snippet that encodes a decision more precisely than prose can, inline only the decision-rich part and state that it came from a prototype.

## Testing Decisions

Testing decisions already established. Include:

- the public seam(s) that tests should exercise;
- what makes a good test for this feature: external behavior, not implementation details;
- relevant prior test patterns in the codebase;
- edge cases and error behavior discussed.

## Out of Scope

Things explicitly ruled out or implied out of scope by the settled conversation.

## Further Notes

Open questions, assumptions, risks, or missing context. Do not turn these into an interview during synthesis.
