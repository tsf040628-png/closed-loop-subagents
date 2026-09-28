# Configuration

## Configuration layers

Resolve settings in this order:

1. Host-admin or organization policy.
2. Explicit instructions for the current run.
3. The user's saved host-specific profile.
4. The host adapter's documented defaults.
5. Skill defaults.

Do not allow a project file to override a stronger host policy or a current-run user instruction. Record effective values in the run snapshot before dispatch.

## User-adjustable settings

- Role model and reasoning effort for Planner, Executor, Reviewer, and Planner escalation.
- Whether to require actual runtime model telemetry. Assignment auditing remains separate.
- Review-cycle cap from 1 to 8. Raising the cap after it is reached requires explicit user approval and a bounded extension.
- GitHub prior-art search scope and minimum stars. Default: technical tasks and 1,000 stars.
- Log detail. Default: compact final log while preserving plans, artifacts, evidence, review decisions, model-assignment records, and cleanup receipts.
- Output language. Default: match the user.
- Clarification mode. Default: use grilling when available and the built-in frontier-question method otherwise.

The three-role separation, independent review, evidence-based status, no false model claims, and user authorization boundaries are workflow invariants.

## Model resolution

The profile stores host-native model identifiers. The adapter translates generic fields into the host's native format, such as model plus effort, model plus reasoning effort, or provider/model.

The YAML example is a logical profile, not a native configuration file for every host. Values such as `current_session` and `inherit` are sentinels: adapters translate them to the host's documented inheritance behavior and must not pass the sentinel as an actual model ID.

Codex default profile:

- Planner, Executor, and Reviewer: gpt-6-luna / max.
- Planner escalation: gpt-6-sol / medium.
- Earliest escalation: corrective Planner cycle 4.
- Decision maker: independent Reviewer; upgrade only for a serious planning or route error.

Other hosts ask during setup for their available model names and reasoning controls. Their profiles intentionally avoid hard-coding a vendor catalog that may not exist on the user's plan, region, edition, or model provider. The recommended starting point is that host's active session model for each role; the user can select a distinct model and effort per role plus a separate Planner escalation model. Save the user's exact host-native identifiers in that host's profile. Do not assume cross-platform model names are interchangeable or invent one when the host's model picker is unavailable.

If the host cannot assign a model per subagent, ask whether the user accepts session-model inheritance. If the user requires separate assignment, report the host limitation and do not claim compliance. If the host accepts assignments but does not report actual runtime models, mark runtime telemetry unavailable.

## Example

See ../assets/config.example.yaml for the shared configuration example. The file is a template; a host adapter may store the user's completed profile in a different user-level location.
