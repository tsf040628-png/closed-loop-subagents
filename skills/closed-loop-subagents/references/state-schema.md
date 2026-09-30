# Shared State Schema

Keep versioned run state in the active workspace's scratch area. Do not require Git. The Controller alone updates snapshots and the append-only event log.

## Status codes

| Scope | Values |
|---|---|
| Run | PLANNING, PLAN_REVIEW, PLAN_REWORK, WAITING_FOR_USER, READY, EXECUTING, EXECUTION_REVIEW, EXECUTOR_REPAIR, CONSOLIDATING, ESCALATED, COMPLETE |
| Plan | DRAFT, IN_REVIEW, REVISION_REQUIRED, NEEDS_INPUT, APPROVED, STALE, ESCALATED |
| Step | BLOCKED, READY, IN_PROGRESS, IN_REVIEW, REVISION_REQUIRED, APPROVED, STALE, ESCALATED |
| Review | PENDING, PASS, PLAN_GAP, EXECUTION_DEFECT, ESCALATE |

Do not mark a run COMPLETE while any required review, criterion, artifact, evidence item, or approved cleanup is incomplete, stale, blocked, or unverified.

## Feedback-cycle budget

Initialize feedback_cycles.used to 0 and feedback_cycles.limit to 8. A cycle is one corrective dispatch by the responsible role followed by one independent review. The initial review and a review that passes without correction consume no cycles. User-clarification rounds do not count.

All plan, execution, and consolidation corrections use the same counter. When eight cycles are used and the task remains incomplete, stop additional corrections, report completed work and unresolved findings, and ask the Reviewer to judge whether a bounded extension is likely to help. Continue beyond eight only after explicit user approval and record the new limit and approval in a new snapshot. Never bypass the cap by renaming or splitting work.

## Model policy and evidence

Resolve a host-specific profile before the first dispatch. Record separate values for:

- Requested model and effort sent to the host.
- Assignment accepted or rejected by the host interface.
- Assigned model and effort confirmed by that interface.
- Actual runtime-reported model and effort, if exposed.
- Runtime observability: AVAILABLE, PARTIAL, UNAVAILABLE, or UNKNOWN.

An accepted assignment proves what the interface accepted; it does not prove hidden runtime configuration. If telemetry is unavailable, record RUNTIME_MODEL_UNOBSERVABLE. If a requested assignment is rejected, mismatched, or unconfirmed, record MODEL_POLICY_VIOLATION and escalate.

If the user's saved profile requires actual runtime-model telemetry and the host cannot provide it, do not start the run under assignment-only evidence. Ask whether the user accepts that weaker evidence or prefers a host that exposes runtime telemetry.

The Reviewer decides whether to use the configured Planner escalation model for a serious planning or route error. The earliest eligible Planner correction is cycle 4. The Codex default target is gpt-6.1-sol / medium; other hosts use the exact target saved in their setup profile. Do not make a model upgrade based only on ordinary progress.

## Snapshot example

~~~json
{
  "schema_version": 1,
  "run_id": "stable-run-id",
  "revision": 1,
  "run_status": "PLANNING",
  "host": {
    "id": "codex",
    "surface": "desktop",
    "version": "observed-or-null",
    "adapter_ref": "adapters/codex/README.md",
    "capabilities": {
      "native_skills": "AVAILABLE",
      "independent_subagents": "AVAILABLE",
      "per_role_model_assignment": "AVAILABLE",
      "runtime_model_telemetry": "UNKNOWN"
    }
  },
  "goal": {
    "summary": "User goal",
    "input_ref": "inputs/goal-v001.md",
    "requirements": []
  },
  "authorization": {
    "allowed_actions": [],
    "verification_requested": false,
    "external_actions_authorized": []
  },
  "configuration_ref": "config/closed-loop-subagents-v001.yaml",
  "model_policy": {
    "role_defaults": {},
    "planner_escalation": {},
    "upgrade_decisions": [],
    "dispatches": [],
    "compliance_status": "UNVERIFIED"
  },
  "feedback_cycles": {
    "used": 0,
    "limit": 8,
    "authorized_extensions": []
  },
  "user_input": {
    "status": "NONE",
    "question_round": 0,
    "frontier": [],
    "confirmation_ref": null
  },
  "plan": {
    "status": "DRAFT",
    "revision": 1,
    "artifact_ref": "plans/plan-v001.md",
    "prior_art_ref": null,
    "review_records": []
  },
  "steps": [],
  "final_reviews": {
    "acceptance": {"status": "PENDING", "record_ref": null},
    "consolidation": {"status": "PENDING", "record_ref": null},
    "closure": {"status": "PENDING", "record_ref": null}
  },
  "consolidation": {
    "status": "NOT_STARTED",
    "canonical_log_ref": null,
    "cleanup_manifest_ref": null,
    "cleanup_receipt_ref": null
  },
  "escalation": null,
  "latest_event_sequence": 0
}
~~~

Only store facts known from the task. Use null, empty lists, or UNKNOWN rather than inventing information. Every review record cites exact immutable input revisions. Every model dispatch records the host adapter, role, cycle, requested settings, assignment outcome, assigned settings, runtime-reported settings if available, and Reviewer decision for any escalation.

## Event records

Use monotonically increasing sequence numbers. An event records timestamp, actor, transition, plan and step revisions, feedback-cycle number, verdict, finding references, artifact and evidence references, dispatch method and outcome, requested and assigned model settings, runtime-reported settings when available, and concise factual notes.

## Plan revisions and stale steps

Substantive changes to scope, criteria, dependencies, permissions, or execution order create a new immutable plan revision and require independent plan review. Mark affected completed or active steps STALE; mark dependent downstream steps STALE as well. Keep earlier artifacts and reviews as history. Do not carry old approval forward without a documented impact analysis.

## Escalation

Set the run to ESCALATED when required delegation or permissions are unavailable, an unresolved requirement would require a guess, requested model assignment cannot be accepted or confirmed, the cycle cap is exhausted before completion, or an authorized acceptance criterion cannot be verified. Record the blocker, affected requirement, completed work, and exact input or access needed.
