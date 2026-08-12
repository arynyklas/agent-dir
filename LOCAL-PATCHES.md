# Local patches to vendored skills

Skills under `skills/` are vendored from upstream (`.openskills.json` records the source
commit) and get overwritten by `openskills` sync. Every deliberate local deviation is listed
here — re-apply it after a sync.

## skills/grilling

- Upstream: `mattpocock/skills` @ `84fdeffd12f2ee307994d1eb6feb48173b6e0502`, `skills/productivity/grilling`
- Patch (2026-08-12): each round MUST be delivered through the harness `ask` tool in a single
  call (one `questions[]` entry per frontier question, `recommended` = the answer we argue for);
  the upstream `❓ **Q1** … ➡️` markdown format is demoted to a fallback for harnesses with no
  interactive question tool.
- Why: `ask` renders selectable options and collects the whole round in one interaction, so
  answers come back structured instead of as free prose we have to re-parse.
