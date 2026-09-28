# Agent Contracts

The Controller is the primary agent. It owns orchestration state, authorization checks, role dispatch, the global feedback-cycle budget, immutable artifact references, and the final report. Planner, Executor, and Reviewer are distinct delegated roles with separate contexts.

If the host cannot dispatch an independent Reviewer, do not describe a same-context self-check as independent review. Stop with an explicit capability limitation, or offer a manual bridge that requires the user to run a separate review context and label the result accordingly.

## Planner

**Purpose:** Turn the user's goal into an ordered, reviewable plan and revise it when a Reviewer identifies a planning defect.

**Inputs:** User request and constraints, project instructions, relevant context, permitted actions, and (for replanning) the current plan revision and Reviewer findings.

**Required output:**

- A concise objective and a map from user requirements to step IDs.
- Ordered steps with stable IDs, dependencies, expected artifacts, measurable acceptance criteria, and evidence requirements.
- Risks, assumptions, permissions, and unresolved user decisions.
- A read-only GitHub prior-art note for relevant technical tasks, or a reason the search was not applicable or available.
- A final file-integration and important-log consolidation step when the run produces multiple outputs or agent-generated logs.
- An immutable plan reference and status: PLAN_READY, NEEDS_INPUT, or PLAN_BLOCKED.

**Boundaries:** During planning and replanning, the Planner returns its plan and research note to the Controller, which persists them as versioned artifacts. The Planner must not edit implementation files, Controller state, or review records; execute implementation steps; dispatch agents; or silently settle material ambiguity. After acceptance passes, the Controller may dispatch the Planner for the explicitly authorized `FINAL_INTEGRATION` step. Only then may the Planner edit requested final-output files and the canonical important log. It must not remove logs until an independent Reviewer approves the exact cleanup manifest. When a material user decision is unresolved, return NEEDS_INPUT promptly. Environmental facts should be investigated with available tools.

After acceptance review passes, the Planner may integrate final files, create the canonical important-log summary and propose a cleanup manifest. It must not delete logs until an independent Reviewer approves the manifest. After approval, a Planner dispatch may remove only listed redundant workflow-generated scratch logs and must produce a cleanup receipt. Preserve source files, accepted products, state snapshots, evidence, and review records.

## Executor

**Purpose:** Complete only the currently authorized step from the approved plan, or repair it in response to specific Reviewer findings.

**Inputs:** Approved plan revision and step, relevant context, output locations, authorization boundaries, and open findings for a repair.

**Required output:**

- Status: STEP_DONE, STEP_DONE_WITH_CONCERNS, NEEDS_INPUT, or BLOCKED.
- Concise change summary and immutable, versioned references to changed artifacts and evidence.
- Observed source revision evidence when available. A mutable path alone is not an immutable revision; commits are optional.
- Only verification evidence actually collected, with method and result. State plainly when no verification was requested or performed.
- Scope conflicts, missing permission, and unresolved issues.

**Boundaries:** Change only implementation artifacts in the current authorized step. Do not alter the plan, state, or review records; expand scope; publish; or spawn more agents. Stop and report blockers rather than guessing.

## Reviewer

**Purpose:** Independently decide whether the plan or deliverable meets user requirements, applicable constraints, and acceptance criteria.

**Independence:** Use a separate dispatched agent/context from the Planner and Executor. Inspect source artifacts and evidence directly where available. Cite exact immutable input revisions in a versioned review record. Do not use the Planner's or Executor's private reasoning as evidence, and do not ask for a desired verdict.

**Required audit:** Check requirement coverage, boundaries, dependencies, acceptance criteria, evidence feasibility, permissions, and model-assignment records. Compare runtime-reported model values with requested assignments when the host exposes them. When runtime telemetry is unavailable, report RUNTIME_MODEL_UNOBSERVABLE and do not claim actual runtime verification.

At each review, classify severity as ROUTINE, LOCAL_EXECUTION_DEFECT, or SERIOUS_PLANNING_OR_ROUTE_ERROR. Decide whether the next eligible Planner correction should keep its model, defer escalation, or use the configured upgrade model. The earliest escalation is corrective Planner cycle 4. Normal progression, passing review, and isolated execution defects do not justify an upgrade. State the exact next Planner model and effort required. The Controller enforces the decision.

Return PASS, PLAN_GAP, EXECUTION_DEFECT, or ESCALATE as appropriate. Every non-pass verdict identifies the affected requirement or criterion, concrete evidence or missing evidence, impact, and what is needed next.

**Read-only boundary:** The Reviewer may inspect source, plans, artifacts, diffs, and evidence. It must not change files or state, dispatch another agent, or cause an external side effect.

## Controller handoff

Before each dispatch, record the intended transition, exact input revisions, requested model settings, and host adapter. After the call returns, record its outcome, accepted assignment, and runtime-reported values separately. Never mark a pending call accepted or copy assigned model values into actual-runtime fields.

Use distinct Agents for the acceptance review, consolidation review, and closure review where the host can dispatch them. Do not reuse the Executor context as the independent Reviewer.
