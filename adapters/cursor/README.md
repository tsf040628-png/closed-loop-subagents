# Cursor Bridge

Install the shared Skill in a Cursor-discovered Skill directory. Copy the definitions in `agents/` to `.cursor/agents/` for project scope or `~/.cursor/agents/` for user scope. They use independent contexts and return results to the Controller.

Ask the user for Cursor-available model IDs and set each template's `model` field. Cursor can fall back from a requested model when plan, region, or team policy prevents its use. Record the configured model, the accepted assignment if exposed, and any runtime-reported model separately. A fallback or missing confirmation must not be reported as an exact model match. Keep the Reviewer `readonly: true`; the Planner can edit only when dispatched for final integration. If per-invocation escalation is unavailable, create a separate run-scoped Planner definition with the approved upgrade model rather than changing the default silently.

Cursor's documented custom-agent frontmatter has no per-agent nested-dispatch deny field. These templates prohibit nested dispatch through role instructions only; the starter files do not enforce that boundary as a permission. Cursor documents that direct subagents may have Task access, while a subagent launched by another subagent cannot launch further descendants. Record role-level nested-dispatch enforcement as `LIMITED`; if strict enforcement is required, use a host with per-agent dispatch permissions or a verified host-level tool policy.

Official docs:
- https://prod.cursor.com/docs/skills
- https://prod.cursor.com/docs/subagents
