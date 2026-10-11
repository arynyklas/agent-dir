# Local patches to vendored skills

Skills under `skills/` are vendored from upstream (`.openskills.json` records the source
commit) and get overwritten by `openskills` sync. Every deliberate local deviation is listed
here — re-apply it after a sync.

## skills/grilling

- Upstream: `mattpocock/skills` @ `49dd158d1076134a641b33efb035946536778336`, `skills/productivity/grilling`
- Patch (2026-08-12): each round MUST be delivered through the harness `ask` tool in a single
  call (one `questions[]` entry per frontier question, `recommended` = the answer we argue for);
  the upstream `❓ **Q1** … ➡️` markdown format is demoted to a fallback for harnesses with no
  interactive question tool.
- Why: `ask` renders selectable options and collects the whole round in one interaction, so
  answers come back structured instead of as free prose we have to re-parse.

## skills/resolving-merge-conflicts

- Upstream: `mattpocock/skills` @ `3cca18b368ae95cdbdebbff572ccafa662551015`, `skills/engineering/resolving-merge-conflicts`
- Kept (2026-10-12) after upstream deleted the skill in `daa01d8` ("no longer needed", no
  replacement). It stays frozen at `3cca18b`: a sync has no upstream copy to pull, so skip it
  rather than delete it.
