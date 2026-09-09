# Section outlines

Use the declared document type. Mark a section not applicable with a reason when it genuinely
does not apply. A deferral belongs in **What this document does not cover** and names the
receiving artifact, owner, and gate. Do not defer a decision needed to approve this artifact
or execute the current phase.

## Common sections

HLDs, LLDs, and RFCs include:

- **Header** — title, type, status, date, owner, revision, and related work if it exists.
- **Decisions** — stable identifiers such as D1, with rationale, rejected choices, and status.
  Use an anchor for each decision. An LLD may link to the HLD's decision records if it also
  recaps their substance.
- **Risks and open questions** — separate accepted risks from unresolved questions. Each has
  an owner, consequence, and resolution gate; accepted risks also carry an acceptance reason.
- **What this document does not cover** — boundaries and explicit deferrals.
- **Glossary** — expand acronyms on first use as well as collecting domain terms here.
- **Evidence index** — stable IDs, claims supported, source locations or read-only commands,
  revisions or check times, and limitations. Never include secret values.

## ADR — architecture decision record

Keep one decision short enough to review on its own. An accepted record is superseded by a
new record; it is not rewritten to describe a different decision.

1. **Title and status** — proposed, accepted, or superseded; date and decision owner.
2. **Context** — requirements, constraints, and cited facts that make the choice non-obvious.
3. **Decision** — the selected approach and why it satisfies the governing requirement.
4. **Alternatives** — credible choices and their tradeoffs, including why each was rejected.
5. **Consequences** — benefits, costs, risks, constraints, and any follow-up work.
6. **Evidence index and related records** — a compact list of evidence IDs, supported claims,
   sources, revisions or check times, and limitations; add supersession links when applicable.

An ADR does not need the other common sections above. Its compact evidence index still
meets the evidence requirements. Keep unresolved decision questions explicit; a proposed
record with those questions is not accepted.

## HLD — high-level design

Specify the architecture sufficiently for review and detailed design. Exact file contents
and operational commands belong in an LLD; include evidence commands in the evidence index.

### Problem and choices

1. **Purpose and problem** — the driving requirement, measured pain or opportunity, and
   baseline. Label estimates or missing measurements instead of inventing history.
2. **Requirements** — stable IDs, testable outcomes, constraints, and which requirement
   distinguishes the options. Mark assumptions awaiting confirmation.
3. **Current state** — verified topology, interfaces, and constraints relevant to this design.
4. **Options** — viable alternatives with honest gains and losses. For substantial choices,
   compare at least three or explain why fewer are applicable. Include build versus buy for
   commodity scope and explain the selected approach against the requirements.

### Proposed system

1. **Architecture** — components, responsibilities, interfaces, trust boundaries, and a
   diagram. Explain the rationale for each consequential technology choice.
2. **Sizing and cost basis** — workload, throughput, retention, growth, units, headroom,
   assumptions, and arithmetic. Include consequential versions and compatibility constraints;
   unresolved choices that could change the architecture must be decided before approval.
3. **Dependencies** — backing services, ownership, topology, startup ordering, availability
   assumptions, and the parameters governing failure behavior.
4. **Security and data** — classification, access, lifecycle, and isolation boundaries when
   sensitive data is involved; distinguish shared storage from shared authorization.
5. **Operations and failure modes** — detection, blast radius, degraded mode, recovery
   mechanisms, triggers, and responses. Derive recurring costs from the sizing basis.

### Delivery and boundaries

 1. **Extensibility** — accommodate an identified near-term scaling requirement without
    specifying speculative features. Say when none is relevant.
 2. **Delivery** — phases, dependencies, measurable exit gates, and escalation when a gate
    fails. Name the LLD or other artifact that will specify execution.
 3. Include the common decisions, risks/questions, non-scope, glossary, and evidence sections.

## LLD — low-level design

Specify an implementation contract. For a build, focus on components and interfaces. For a
migration, upgrade, or decommission, also make every intermediate state and recovery path
explicit. Justify any change-procedure section marked not applicable.

### Contract

1. **HLD recap** — restate the governing requirements and decisions, with source links.
   If there is no HLD, state the approved requirements and design choices directly.
2. **Evidence baseline** — revisions, versions, resource identities, and observed state used
   to prepare the procedure. Explain what must be rechecked before execution.
3. **Component specifications** — paths, schemas, resource attributes, permissions,
   interfaces, configuration, and algorithms as needed. Include exact contents where
   necessary; do not substitute large copied files for a clear contract.
4. **Coverage** — map every governing requirement to its specification and acceptance check.

### Procedure

1. **Prerequisites** — access, quotas, approvals, merged dependencies, backups, or drained
   queues that must be verified before starting. State the expected result of each check.
2. **Ordering** — dependencies and the reason each consequential ordering constraint exists.
3. **Invariants** — conditions that must hold at every intermediate step, including safe
   interruption states and how they are verified.
4. **Rollback and reversal** — per phase, how recovery works, state compatibility, what is
   irreversible, and the point of no return. If rollback is impossible, specify the recovery
   or forward-fix path and the approval needed to cross that boundary.
5. **Timing** — estimates, maintenance windows, deadlines, and the condition that stops an
   overrun. Include this when disruption or an operational window matters.

### Verification and handoff

 1. **Acceptance** — machine-checkable outcomes per component and for the overall change.
 2. **Verification** — executable checks, target context, expected output, and failure
    interpretation. Distinguish read-only checks from mutation steps; keep secrets as
    references and re-derive temporary paths or observations that may expire.
 3. **Executor handoff** — objective, baseline, ordered steps, dependencies, acceptance
    checks, and stop conditions. State exactly when the executor must stop for a decision.
 4. Include the common decisions, risks/questions, non-scope, glossary, and evidence sections.

## RFC — request for comments

Use the HLD outline and add the agreement-related material below. A discussion draft can
carry explicit questions; an approved RFC records the answers. When composed with
`/planning:planning`, its open-question gate requires resolution before Phase 3.

- **Review request** — owner, reviewers or decision-makers, status, review window, and the
  specific agreement requested. Follow the team's versioning convention if one exists.
- **Build versus buy** — an explicit choice for any commodity portion of the scope, even
  when the decision is to build. Keep specialized requirements separate.
- **Data and integrity contracts** — when the system owns state, specify the data model,
  integrity guarantees, API/access boundaries, and ownership.
- **Migration** — transition from current state, coexistence, cutover, and recovery, with
  detailed execution deferred to an LLD when appropriate.
- **Proposed work** — phases and exit gates for approval. Describing work does not authorize
  creating tickets or implementing it.
- **Alignment record** — once execution begins, record deviations with reasons and dates.

The planning workflow also requires its ten minimum artifact sections, including the
resource-mutation table and next steps. These outlines add detail without removing those gates.
