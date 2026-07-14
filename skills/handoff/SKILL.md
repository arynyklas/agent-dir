---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

# Handoff

Write a handoff document so a fresh agent can continue the work.

## Destination

Save the artifact to:

```text
local://<slug>-handoff.md
```

Use a short kebab-case slug describing the next session's focus. Do not write handoffs into the repository unless the user explicitly asks.

## Content

Include:

- current goal and user intent;
- completed work with evidence;
- current state and open decisions;
- exact next actions;
- verification already run and what remains;
- risks, blockers, or assumptions;
- links/paths to existing artifacts.

If content already lives in a spec, plan, ADR, issue, PR, diff, report, or local artifact, reference that path or URL instead of duplicating it.

## Redaction

Redact secrets and sensitive personal data before writing:

- API keys, tokens, passwords, cookies, private keys;
- credentials or connection strings;
- sensitive personal information;
- private customer data not needed for continuation.

Use `[REDACTED]` and state what kind of value was removed when helpful.

## Suggested skills

Include a “Suggested skills” section only with locally installed skill names that are relevant to the next session. Do not recommend unavailable upstream, personal, deprecated, or removed skills.

## If arguments were passed

Treat arguments as the next session's intended focus and tailor the handoff to that focus.
