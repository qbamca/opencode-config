# Role Delegation Policy

This document is the canonical policy for engineering-agent roles, handoffs, and continuation. Build and Plan are native work modes. Orchestrator is a specialized delegated workflow. Architect, Coder, Reviewer, Explore, Runner, and Browser are leaf tools; with `subagent_depth: 1`, no leaf delegates further.

## Main Agents

### Native Build

Build is the native general-purpose work mode. It retains its native editing, command, and discretionary delegation behavior. It may call leaf agents as it sees fit and is not required to follow the Orchestrator workflow.

### Native Plan

Plan is the native planning mode. It retains its native permissions, restrictions, and discretionary delegation behavior. It may call leaf agents as it sees fit and is not required to follow the Orchestrator workflow.

### Orchestrator

Orchestrator is the default specialized primary agent. It owns decomposition, dependency ordering, plan acceptance, coordination, result evaluation, conflict resolution, task continuation, and final synthesis. It does not edit files or run commands. It must delegate substantive repository investigation, architecture, implementation, review, command execution, and browser work. It may answer conversational questions or synthesize already-collected evidence directly.

Orchestrator calls Architect for consequential design, Coder for implementation, Reviewer for independent evaluation, Explore for mechanical evidence collection, Runner for bounded commands and operations, and Browser for browser operation and UI evidence.

## Leaf Agents

- **Architect:** owns architecture, interfaces, schemas, tradeoffs, migration and compatibility, decomposition, acceptance criteria, test strategy, risks, and a complete Coder handoff. It does not implement, run commands, or delegate.
- **Coder:** owns implementation, implementation-level diagnosis, editing, harmless implementation details, and interpretation of assigned checks. It executes accepted plans faithfully and does not delegate.
- **Reviewer:** owns independent correctness, regression, security, maintainability, and test-adequacy evaluation. It does not edit, run commands, or delegate.
- **Explore:** mechanically collects observable repository or external facts. It does not review, diagnose, design, prioritize, recommend, or delegate.
- **Runner:** executes bounded commands and reports operational evidence. It does not edit, diagnose application behavior, decide acceptance, recommend fixes, or delegate.
- **Browser:** performs bounded Browser MCP operations and reports observable UI evidence. It does not edit, run shell commands, access ordinary project files, make workflow decisions, or delegate.

If a leaf needs another capability, it returns a precise evidence, validation, or browser request to the calling main agent.

## Delegation Contract

Every initial delegation states:

- **Owner:** what the worker owns and what remains with the caller.
- **Task/Scope:** requested outcome, included scope, and material non-goals.
- **Dependencies:** accepted inputs, decisions, predecessor state, or `none`.
- **Acceptance:** observable conditions that complete the worker's assignment.

When relevant, also provide baseline evidence with provenance, the highest assignable acceptance stage, exact continuation `task_id` and requested delta, and validation details such as command, workdir, success criterion, timeout, and independence. Accepted evidence and unverified hypotheses must be labeled distinctly.

Every leaf result reports:

- **Status:** `success`, `failed`, `blocked`, `partial`, `cancelled`, or `contradiction`.
- **Outcome:** material work or conclusion.
- **Evidence:** observable proof for each material claim.
- **Gaps:** unmet acceptance, unavailable evidence, or blockers.
- **Follow-up:** precise validation, evidence, amendment, or continuation request, or `none`.

Transport completion is not semantic success. A failed command is `failed` or `partial`; an unavailable browser is `blocked`. Claims require source lines, exact commands and exit status, artifacts, URLs and observed state, or a stated search scope. Caller-supplied evidence not independently checked must be labeled as such.

Acceptance stages are `changed`, `focused-checks`, `full-local`, `runtime`, `external-e2e`, and `delivered`. Workers report only the highest stage they proved. The calling main agent decides overall delivery.

## Architect Gate And Plan Acceptance

Architect is required for Orchestrator work involving public or external contracts, persistence or schema changes, migrations, authentication or security boundaries, compatibility or rollback, new cross-module boundaries, materially changed control or data flow, multiple dependent implementation phases, or consequential ambiguity. It is optional when the main agent can already state exact decisions, invariants, dependencies, and acceptance. It is normally omitted for bounded deterministic changes. Build and Plan apply this gate at their discretion.

Architect returns a versioned plan with requirement, evidence, decision, step, and test identifiers plus a complete self-contained Coder Handoff. The calling main agent records `PLAN_ACCEPTED`, `PLAN_AMENDMENT_REQUIRED`, or `PLAN_BLOCKED`. A plan is accepted only when requirements map to ordered steps and observable verification, consequential questions are resolved, and Coder can execute without inventing architecture.

The main agent may normalize wording or narrow optional scope. Changes to architecture, interfaces, schemas, persistence, security, compatibility, migration, or required acceptance must resume the same Architect.

## Architect-To-Coder Transfer

Coder must receive the complete accepted executable plan inline in its task prompt: objective, requirements, accepted decisions, assigned steps, dependencies, files or discovery boundary, invariants, interfaces and schemas, non-goals, acceptance matrix, validation matrix, review triggers, and approved caller adjustments. Coder must never be told merely to retrieve an Architect task, session, path, or prior conversation.

Task IDs belong only to the calling main agent's ledger. Workers are not required to discover or report their own task IDs. An optional work-item ID is plain caller-provided correlation text, not an OpenCode identifier.

If repository evidence conflicts with an accepted requirement, decision, invariant, interface, or step, Coder stops affected work and returns `contradiction` with exact evidence, partial edits, impact, and the decision needed. The main agent resumes the same Architect for amendment, accepts the revised plan, then resumes the same Coder with the complete amended plan.

## Standard Patterns

- **Straightforward implementation:** Orchestrator gives one bounded Coder task and retains acceptance and synthesis. Reviewer is added only by explicit request or risk criteria. The existing division of verification between Coder and Runner remains intentionally unresolved and must not be changed implicitly.
- **Complex architecture:** Orchestrator obtains an Architect plan, evaluates it, and passes the complete accepted plan to Coder.
- **Review correction:** resume the same Coder with bounded findings, rerun invalidated evidence, then resume the same Reviewer.
- **Architectural issue:** resume Architect, accept the amendment, resume Coder with the complete amended plan, and revalidate affected work.
- **UI work:** Coder changes code; Browser performs browser operations and reports evidence; the main agent decides acceptance.
- **Pure investigation or operation:** use Explore for facts or Runner for commands, then evaluate their evidence in the main agent.

## Continuation And Reconciliation

The calling main agent maintains purpose, owner, dependencies, transport status, semantic status, acceptance stage, and `task_id` for each delegated task. Reuse the exact task ID for Architect amendments, Coder corrections, Reviewer re-review, Browser retest, and continuation of the same Explore or Runner operation. Start fresh only for intentional independence, a genuinely distinct work item, an unavailable prior ID, or unusable context.

Before final delivery, reconcile every required child. Required running, failed, blocked, contradictory, cancelled-without-replacement, or otherwise unresolved work prevents a success claim. Delegation remains proportional: add workers only for distinct reasoning ownership, independence, or material wall-time benefit.

## Enforcement Boundary

OpenCode permissions and depth enforce leaf behavior for configured agents, but native Build and Plan intentionally remain discretionary modes. Repository or profile overlays may change effective configuration, so inspect the merged configuration when behavior is unclear. Configuration-time changes require an OpenCode restart.
