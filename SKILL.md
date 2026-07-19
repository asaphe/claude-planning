---
name: planning
topic: Planning
description: >-
  Phase-gated workflow for design/RFC/remediation-planning work: restate the goal, dispatch
  parallel research before any solutioning, produce a draft artifact, resolve every open question
  interactively before finalizing, write the RFC as a full, complete, standalone doc (never a
  private chat-only link) — published to an external tool only if the user explicitly asks,
  never forked into a duplicate when it is — get explicit approval before creating tickets or
  touching code, then sequence execution as a parent epic + research-only ticket first. Always
  maximum rigor with a task-appropriate expert persona chosen automatically — not a togglable
  mode. Distinct from a code/PR review skill (reviews something that already exists) — this
  produces the plan/RFC in the first place. Invoke proactively for any open-ended design/RFC/
  remediation-planning ask, including long freeform prompts that never say the word "plan".
  Usage - /planning [freeform goal/problem statement, or paste context — args are optional]
user-invocable: true
allowed-tools: Agent, Read, Glob, Grep, Write, Edit, Artifact, AskUserQuestion, WebFetch, WebSearch, Bash(mkdir *), Bash(git *), Bash(gh *), Bash(date *), Bash(cat *), Bash(ls *), Bash(jq *), Bash(grep *), Bash(find *)
argument-hint: "[freeform goal/problem statement, or paste context — args are optional]"
---

# Planning

A phase-gated pipeline for producing a design/RFC/remediation plan, distilled from a recurring
pattern of corrections seen across many planning sessions: don't solve before researching, ask
the open questions instead of assuming, a draft artifact isn't the final RFC, read the thing
before judging it, don't fork a duplicate doc, show the plan before executing it.

This skill **produces** a plan — it is not a review of one that already exists (that's a
council/multi-perspective-review skill, if this repo has one) and not a review of merged code
(that's `/pr-review`, if this repo has one).

## When to use

- Any new-service / new-system design, architecture RFC, or cross-repo design decision
- Security/compliance audits that will end in a remediation plan
- Large research-heavy initiatives before a project epic gets created (ClickUp, Linear, GitHub
  Issues, etc.)
- A remediation or migration plan for an existing system
- **Invoke this proactively** — not only when the user types "plan" or "/planning". A long
  freeform message describing a goal, a problem to solve, or "how should we build X" is
  planning-shaped even without the word. Recognize that and self-invoke; don't wait to be asked,
  and don't re-derive this checklist from scratch inline instead of using it.

## When NOT to use

- Active production incident — unblock first, come back here for the durable remediation once
  the fire's out. Fix in foreground, plan/audit in background — don't gate an active unblock on
  running this whole pipeline first.
- Reviewing an artifact someone else (or a past session) already produced, with no new research
  needed — use a review/council-style skill instead, if one exists in this repo.
- Reviewing merged or open PR code — use `/pr-review`, if this repo has one.
- A single mechanical edit with an obvious correct answer — this pipeline is overhead, not
  rigor, on a one-line change.

## Persona and rigor — not a mode, the default

This skill runs at **maximum rigor on every invocation** — there is no lighter setting. What
varies is the *persona*, not the *effort*: at Phase 0, silently adopt whichever domain expert
the goal calls for (staff/principal architect for a system design, CISO + security researcher
for a security audit, FinOps lead for a cost initiative, DBA for a schema migration) and hold
that posture through every phase. Don't ask the user to name it — infer it from the goal
statement; only surface it if the goal genuinely straddles two unrelated domains. "Maximum
rigor" means an extra verification pass at each gate below, not a stronger-worded assertion of
confidence — a plan is not more done because it says "production-grade" more confidently.

## Composes with

- **A structured multi-perspective review**, if this repo/team has one, for a Phase 2/3 decision
  point that's itself load-bearing: an RFC that's unusually large, a vendor choice with lock-in,
  a migration's sequencing. That kind of review checks a fixed artifact; this skill is what
  produces the artifact it would check.
- **Model/agent escalation** — if your tooling supports routing to a stronger model or a
  deep-research mode for Phase 1 research and Phase 2 design synthesis, use it there and drop
  back to a standard model once execution starts. Don't ask permission to escalate — name the
  escalation and proceed.
- **Artifact format** — prefer HTML for anything a human will read/review, especially if it's
  long or comparison-heavy; prefer Markdown for anything mainly meant to be re-read by an agent
  across sessions (tracker/handoff notes, research findings). When both a human and an agent
  need a document that *persists and gets revisited across sessions*, keep the working source in
  Markdown and generate an HTML rendering at review checkpoints rather than in lockstep on every
  edit. This doesn't cover Phase 2's live draft below — that's real-time interactive iteration
  with the human actively reviewing in the same session, not a cross-session document, so HTML
  throughout is the right call there, not a contradiction of this heuristic. If the user names a
  format, that wins outright, no heuristic needed.

---

## Phase 0 — Goal

Restate the driving requirement in one paragraph before touching solution options. This is the
anchor every later option gets evaluated against — not a formality. If the user already stated
the goal in detail, play it back to confirm you have it right rather than silently assuming; if
it's implicit, state your read of it and let them correct it before Phase 1 starts.

## Phase 1 — Research (gate: no solving before evidence)

**Nothing gets designed or solved until this phase produces evidence.** For anything
non-trivial, dispatch parallel `Agent` calls per topic/system rather than researching serially —
each comes back with cited, verifiable findings, not summary-from-memory. A `file:line` citation
only counts as evidence when the file was actually read fresh in this pass (a live `Read`, or a
fetched doc) — not recalled from memory or asserted without opening the file. For large/complex
research (a full secrets audit, a multi-system inventory), plan the research execution itself
first — chunking/batching API calls, sequencing lanes — before running it; say so explicitly if
the task is large enough to warrant that.

Exit criteria: every claim in the findings is either (a) cited from something actually read or
run in this pass — a live-state command (`aws ... describe/get`, `kubectl get`), a fetched doc,
or a `file:line` from a file read fresh just now — or (b) explicitly flagged as unverified/open.
Reciting a `file:line` from memory without having reopened it, a stale local clone, and PR
descriptions are claims to verify, not evidence themselves — the test is "did you just read
this," not "is it a codebase citation vs. a live command."

## Phase 2 — Design draft

Produce a working artifact — HTML, one you expect to revise, not the final form, structured per
§ Artifact template below. This is the live-interactive-review case § Composes with carves out,
not the cross-session-persistence case, so it stays HTML throughout rather than a Markdown
source rendered occasionally. If the `artifact-design` skill is available in your setup, don't
invoke it via the `Skill` tool from inside this skill's own instructions — that self-chaining
path is unreliable (see `learning-loop`'s skills for the same documented gotcha). Instead `Read`
its file directly and apply its guidance before writing the draft. Two things the evidence is
explicit about:

- **Extensibility seam, not speculative build-out.** If there's a near-term scaling axis
  (multi-region, multi-tenant, higher load), confirm the design leaves a cheap seam for it —
  don't build the seam out now, just don't paint yourself into a corner that makes it expensive
  later.
- **Verify before judging.** If the design responds to or critiques an existing artifact/PR/
  proposal, read it in full first. "So you didn't read the artifact?" is the failure mode this
  exists to prevent — summarizing from a title, a description, or a partial fetch is not
  reading it.

## Artifact template — what belongs in the draft and the RFC

Both the Phase 2 draft and the Phase 3 RFC are the same shape at different completeness — the
draft can leave rows open, the RFC cannot. Minimum sections:

1. **Header** — title, status (draft / RFC / approved), date, owner, related ticket(s).
2. **Goal** — the Phase 0 restatement, verbatim or lightly edited.
3. **Context** — what's true today, cited from Phase 1 findings, not restated from memory.
4. **Options considered** — ≥3, each with the tradeoff stated honestly: what it wins AND what
   it loses, not just why the chosen one is best. "A wins on X, loses on Y; B wins on Y, loses
   on X; C wins on Z but costs more setup; chose A because X matters more here" — not "A is the
   best approach," and not a two-option A-vs-B writeup even if honestly argued — the third
   option is what's checked at the Approval gate, not just the tradeoff phrasing.
5. **Chosen approach** — the decision plus rationale tying back to the Goal.
6. **Extensibility seam** — the near-term scaling axis and how the design keeps it cheap, when
   applicable; if none applies, say so in one line rather than omitting the section.
7. **Risks** — concrete named failure modes, not "potential challenges" — with a mitigation or
   an explicit "accepted, here's why."
8. **Resource-mutation table** — for anything destructive/state-changing, per § Gate — Approval's
   shape; if nothing in the plan mutates shared state, say so in one line rather than omitting it.
9. **Open questions** — must be empty by the time this is the RFC; a draft that still has open
   rows is not ready to become one.
10. **Next steps** — the concrete plan the Approval gate reviews.

A one-line status update ("today it's single-region, multi-region should be a cheap seam")
doesn't need all ten sections — use judgment on a draft's completeness at any given moment. The
RFC does need all of them present, but "present" includes an explicit one-line N/A for a
section that genuinely doesn't apply (§6, §8) — that's a filled-in section, not an open row.
"Open" means unresolved/TBD, not "not applicable." It has to stand alone for a reader who
wasn't in the room, whether that reader finds content or a stated reason there's none.

## Gate — Open questions

**Do not proceed to the RFC with unresolved questions.** Surface every open question explicitly
via `AskUserQuestion` rather than assuming an answer and moving on — this is the single most
repeated correction in the source evidence. Batch independent questions into one
`AskUserQuestion` call (most implementations cap around 4 per call — split across multiple calls
if there are more); ask interdependent ones one at a time, waiting for each answer before the
next. Before moving to Phase 3, do an explicit self-check: "are all open questions answered, is
everything actually designed?" — not just "did I ask some questions."

## Phase 3 — RFC

The RFC is a different artifact from the Phase 2 draft, not a renamed copy of it:

- **Full and complete** — every section in § Artifact template is filled in, no open rows; it
  must stand alone and cannot say "see the artifact" pointing at something only one person can
  open. This applies whether or not it's ever published anywhere beyond your local/repo copy.
- **Local by default; publishing to an external tool requires an explicit ask.** The durable
  local/repo copy (see § Composes with — Artifact format) *is* the RFC. Default to leaving it
  there. Only publish to a ClickUp Doc, a wiki, or any other third-party surface when the user's
  own words clearly ask for that — "post this," "share it with the team," "put this in ClickUp."
  Reaching this phase is never itself that request, and neither is vague enthusiasm ("let's get
  eyes on this," "looks good") — treat anything short of a clear ask as staying local, no
  separate judgment call needed.
- **A clear ask without a named surface still needs one more answer: which surface.** "Share it
  with the team" confirms *that* you're publishing, not *where*. When the ask is clear but the
  destination isn't, ask which surface (ClickUp Doc, wiki, whatever this team's shared-doc
  convention is) rather than guessing — that's a mechanical follow-up on a decision already
  made, not a re-litigation of whether to publish at all.
- **When publishing is requested: one canonical copy, not a private link.** If the user does ask
  to share it externally, use a surface colleagues can actually open (not a chat link or a
  personal AI-tool session link), and if an RFC on this topic was already published there,
  **update it in place** — never create a second doc, never title a fix "(corrected)". The same
  amend-don't-fork rule applies to the local copy too: amend it as findings change rather than
  bolting on an "Updates" section as a changelog. If a sub-topic grows too large to amend
  cleanly, split it into its own linked doc.
- **Named-standards framing for security/audit-flavored research.** This specializes the
  template above, it doesn't drop any of its ten sections — Header, Goal, Extensibility seam,
  Risks, Resource-mutation table, Open questions, Next steps, and Chosen approach all keep their
  normal form; only Context and Options-considered get restructured as: tables (grouped by
  pattern, not exhaustively enumerated — collapse near-duplicates) → exceptions →
  recommendations anchored to named standards (OWASP / SOC2 / AWS Well-Architected, as
  applicable) → an Eval/Score section grading current-state posture, in place of plain prose.
  The ≥3-alternatives-with-honest-tradeoffs bar from § Artifact template item 4 still applies
  inside that restructuring — an audit RFC needs ≥3 candidate remediation approaches somewhere
  in its recommendations, not a single prescribed fix per finding, or it's skipped the
  Options-considered requirement by relabeling it. This is not the default shape for every RFC —
  a pure architecture-options doc doesn't need an Eval/Score section — but don't skip it when the
  audit-report shape actually fits.

## Gate — Approval

This gate governs the *next-steps plan*, not the whole skill — Phase 2's interactive draft and
the Open-questions gate both involve the user earlier, as intended. Before *this* plan is
presented, it must clear these on paper first, not after presenting:

- **Resource-mutation table for anything destructive/state-changing:**
  `Resource | Blast-radius classification | Verified-where (live ref) | Status`. Any row
  reading "unverified/unknown/TBD/needs investigation" means the plan is not ready — close the
  gap or ask a specific question instead of handing back an open row.
- **Blast-radius classification** — whatever scheme your infrastructure actually uses (tenant
  tier, environment, region, prod-vs-staging) — for anything touching shared resources comes
  from your live source of truth (a deployment registry, IaC state, a config service), never
  from memory. Misclassifying blast radius is the highest-cost error in this category.
- **≥3 alternatives** considered whenever comparing approaches — no silent collapse to a
  user-named A/B without at least naming what a C would look like.
- **≥1 adversarial failure mode per major step** — actively try to break the plan on paper
  before presenting it, not just list happy-path steps.

Then present the concrete next steps — what tickets, what changes, what order — and stop. Wait
for an explicit yes before creating tickets or touching code. "Looks clean, present the next
steps so I approve before execution" is the literal ask this gate exists for; don't read a
design sign-off as execution authorization, and don't run the checks above *after* approval —
by then it's too late for them to change what got presented.

## Phase 4 — Execution

- **Ticket sequencing:** create a parent epic + a research-only ticket first. Do not
  pre-generate the full ticket breakdown before the research/design phases above have actually
  run. Additional tickets get created once scope is fully known, not speculatively.
- **Executor defaults to Claude.** Optimize the plan for programmatic verification, not a
  sequence of human checkpoints — Claude is running this, not handing off steps for the user to
  execute manually, unless the task specifically requires a human hand (destructive prod op,
  credential rotation the user must perform).
- Persist a tracker/handoff note as tickets/phases close — a ticket comment, a repo doc,
  whatever session-continuity convention this team already uses — so a resumed session doesn't
  re-derive state that was already decided.
