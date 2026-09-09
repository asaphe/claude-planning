# claude-planning

Two complementary skills for architecture work:

- **[Planning](skills/planning/SKILL.md)** runs the process: research, draft,
  resolve open questions, prepare the RFC, get approval, and sequence execution.
- **[Architecture documents](skills/design-doc/SKILL.md)** authors or reviews
  HLDs, LLDs, RFCs, and ADRs using evidence requirements, document outlines,
  formatting guidance, and a readiness rubric.

Use planning for an open-ended problem. Use design-doc when the deliverable
is a document or a review of one. Planning reads design-doc at its draft and
RFC phases; research and execution approval remain planning's responsibility.

## Install

```text
/plugin marketplace add asaphe/claude-planning
/plugin install planning@claude-planning
```

## Usage

- `/planning:planning [goal or problem statement]` — runs the full pipeline.
- `/planning:design-doc author [topic]` — drafts an architecture document.
- `/planning:design-doc review [path]` — reviews an existing document without editing it.
- No hooks, no external config to scaffold — self-contained on install.

Both skills live under `skills/`; the existing `/planning:planning` command
is unchanged. The new skill includes [document outlines](skills/design-doc/references/spines.md),
a [review rubric](skills/design-doc/references/rubric.md), and
[formatting guidance](skills/design-doc/references/formatting.md).

Document readiness is specific to its purpose: an HLD supports architecture
review, while an execution-ready LLD specifies implementation and verification.
Unverified claims remain explicit gaps. Completing or reviewing a document
does not authorize publishing it or executing the proposed work.

## Contributing

Validate the manifest locally before pushing:

```sh
claude plugin validate . --strict
```

Follow the [Claude Code plugin layout](https://code.claude.com/docs/en/plugins-reference#skills).
Check Markdown formatting, frontmatter, and relative links for edited files.
