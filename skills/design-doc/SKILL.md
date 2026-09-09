---
name: design-doc
description: >-
  Author or review architecture documents: high-level designs (HLDs), low-level designs
  (LLDs), requests for comments (RFCs), and architecture decision records (ADRs). Select
  the document type, verify consequential claims, apply its section outline, and report
  what readers still need to know. Use for writing a document or critiquing an existing
  one. For an open-ended problem requiring research, decisions, and execution planning,
  use planning:planning; this skill owns the document, not that process.
user-invocable: true
argument-hint: "[author|review] [topic, document path, or freeform request]"
---

# Architecture documents

Produce documents that let their intended readers proceed without inventing consequential
facts. An HLD must support architecture review and an LLD author's next step; an LLD must
support implementation. An RFC supports agreement, and an ADR records one decision.

## Start with the request

1. Identify **author** or **review** mode. Review mode produces findings, not document edits,
   code changes, infrastructure mutations, or published comments.
2. Read the supplied document in full, including tables, appendices, and diagram labels.
   If a source is inaccessible or incomplete, identify the missing material and limit the
   verdict accordingly. A title or summary is not a substitute for the document.
3. State the artifact type and intended reader. Use the routing table below. When reviewing,
   grade against the declared type; separately flag a mismatch with the requested outcome.
4. Read [section outlines](references/spines.md). In author mode, also read the
   [review rubric](references/rubric.md) before delivery. Use it directly in review mode.

| Type | Question it answers | Completion test |
| --- | --- | --- |
| ADR | Why did we choose this approach? | A maintainer can recover the decision and its consequences. |
| HLD | Is this the right architecture? | A reviewer can challenge the shape; an LLD author can start. |
| LLD | Exactly how will we build or change it? | An executor has specifications, checks, and stop conditions. |
| RFC | What agreement are we asking for? | Reviewers can assess the proposal, alternatives, and requested decisions. |

When asked for both HLD and LLD, create separate linked artifacts. Restate the HLD decisions
in the LLD so it stands alone. A short ADR does not need a full system-design template.

## Evidence requirements

Apply these checks to claims that affect a decision, implementation, cost, or failure behavior.
Match the source to the claim: code proves configured behavior at a revision; an API query
proves observed state at a time. Neither automatically proves the other.

| Check | Required evidence |
| --- | --- |
| Orderable | Verify proposed commercial products, tiers, and instance types against a current vendor catalog, including relevant region or plan restrictions. |
| Versioned | Identify consequential component versions, verify published packages or images, and name both sides of a compatibility claim. |
| Derived | Show the workload assumptions and arithmetic behind sizes, capacity, and cost; include units, region, and pricing date where applicable. |
| Named | Resolve references such as "the pilot" or "the shared cluster" to an unambiguous target. |
| Decomposed | Expand integration verbs into producers, consumers, interfaces, transformations, and failure handling. |
| Failure-determining | State the parameters that control failure: quorum, timeouts, retry limits, consistency, and recovery behavior as relevant. |
| Cited | Attach a source to each consequential factual claim and collect the sources in an evidence index. |
| Marked | Separate verified facts, proposed choices, assumptions, and unresolved questions. Never present an unchecked assumption as observed state. |
| Grounded | Check claims about existing resources through read-only live queries; include actual capacity constraints and allocations when claiming headroom. |

Read primary documentation and inspect the relevant code or read-only APIs. Never mutate a
system to obtain evidence for a document task. If access is unavailable, mark the claim
unverified and name the check needed; do not fabricate a result or claim the evidence bar passed.

Check **every** cost-table row, named commercial product, consequential version/compatibility
claim, and claim about existing infrastructure. Verify other consequential claims too;
sampling background context does not justify declaring unexamined claims verified.

Use stable evidence IDs. Record the source location or command, revision or observation time,
and what it establishes. Include limitations. Keep credentials, private payloads, and
identifiers unsuitable for the intended audience out of the document and its evidence appendix.

## Authoring

Use the selected outline and keep detail proportional to its reader's next action:

- State measurable requirements and the problem they solve. Use verified measurements where
  available; label estimates and missing baselines. Never invent an incident to justify work.
- Compare at least three viable approaches for a substantial choice, including retaining the
  current approach when viable. If fewer exist or the user limits scope, explain that boundary
  rather than inventing an alternative. Evaluate build versus buy for commodity components.
- Explain technology choices per component. Existing familiarity can be a benefit, but show
  its tradeoff against the requirements.
- Cover failures, dependency ordering, degraded operation, and affected users. A mitigation
  names a mechanism, trigger, and response; monitoring alone leaves the response unspecified.
- Describe data classification and isolation whenever customer, tenant, or identity data is
  involved. A diagram must show the same boundaries as the prose.
- Specify enforceable acceptance checks where useful. Proposing a validator or CI gate does
  not authorize implementing it.

## Readability and format

Follow the user's format and repository conventions. Otherwise prefer Markdown for durable
source and execution specifications; use HTML when a visual review benefits from it. Read
[document formatting](references/formatting.md) when producing or reviewing HTML.

Use headings, short paragraphs, comparative tables, and stable decision anchors. Put a
decision or question above its rationale or disposition, rather than squeezing both into
narrow table columns. Expand acronyms on first use and collect domain terms in a glossary.

Include architecture and data-flow diagrams where they clarify structure; add a sequence
diagram where ordering matters. Check every label and boundary against the prose. A simple
ADR may need no diagram. Describe any rendering checks actually performed and their limits.

## Lifecycle and delivery gates

Maintain one canonical source per artifact. Correct its body in place; a generated rendering
is a derivative, not a second editable authority. Follow an established versioning convention,
or agree one before creating parallel versions. Record implementation deviations with reasons.
For accepted ADRs, propose a superseding record instead of rewriting the accepted decision.

Before delivery, produce a **missing-facts list**: what this artifact's intended reader still
needs to decide, verify, or invent. Give each item a consequence, owner, and resolution gate.
Do not assign a person without a basis; an unassigned owner is itself unresolved.

- A **draft** may contain explicitly owned questions. Deliver it as a draft, with a bounded
  review verdict, rather than claiming it is ready for execution.
- An **RFC for discussion** may ask questions explicitly; an approved RFC must record their
  answers. In the full planning workflow, its stricter open-question gate applies before
  Phase 3: use this skill's draft state until that gate is satisfied.
- A **final HLD** may defer implementation details to a named LLD and owner when they do not
  affect architecture decisions. Record deferrals in the non-scope section.
- An **execution-ready LLD** has no unresolved execution-blocking questions. A deferred
  prerequisite blocks the relevant phase until verified; it is not permission to improvise.

In author mode, self-check against the rubric. Before calling a consequential document ready
for execution, request an independent review using the document, its cited evidence, and the
rubric, without supplying your preferred verdict. Use a separate reviewer if available; if
not, report that the independent review is outstanding. Resolve findings before approval.

Deliver the document or review with its readiness verdict, missing-facts list, evidence
limitations, and first blocked next step, if any. A review recommends changes; it does not
approve execution on the user's behalf.

## Composition and scope

The bundled [planning skill](../planning/SKILL.md), invoked as `/planning:planning`, owns
research sequencing, open-question resolution, approval, and execution planning. At its
draft and RFC phases, read this skill and the relevant references for document quality;
its process gates still apply.

Standalone `/planning:design-doc` does not require a ticket tracker, a particular cloud,
private format skills, or another plugin. If authoring reveals an unsettled design decision,
surface it explicitly or use the planning workflow when requested; do not silently decide it.

Save to the requested local or repository location. Finishing a document does not authorize
external publication, ticket creation, code changes, or an audit of unrelated systems.
When sharing is requested, confirm an unspecified destination and update an existing shared
copy where appropriate. Check that recipients can access it and that its contents suit them.
