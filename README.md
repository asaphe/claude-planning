# claude-planning

Phase-gated workflow for design/RFC/remediation-planning work: restate the
goal, research before any solutioning, produce a draft artifact against a
fixed template, resolve every open question interactively before finalizing,
write the RFC as one canonical shareable doc, get explicit approval before
touching tickets or code, then sequence execution as a parent epic plus a
research-only ticket first.

## Install

```
/plugin marketplace add asaphe/claude-planning
/plugin install planning@claude-planning
```

## Usage

- `/planning:planning [goal or problem statement]` — runs the full pipeline.
- No hooks, no external config to scaffold — self-contained on install.

## Contributing

Validate the manifest locally before pushing:

```
claude plugin validate .
```
