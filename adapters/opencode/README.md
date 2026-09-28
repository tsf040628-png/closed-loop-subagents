# OpenCode Bridge

Install the shared Skill under `.opencode/skills/closed-loop-subagents/` or a documented compatible Skill location. Copy the three starter agents from `agents/` to `.opencode/agents/` for project scope or `~/.config/opencode/agents/` for user scope. Each is a separate `mode: subagent` agent.

Ask for provider/model identifiers and per-role reasoning effort. A missing `model` field makes a subagent inherit the invoking primary agent's model; add the selected `provider/model-id` and supported `reasoningEffort` to each agent during setup. Do not assume model IDs or effort names transfer from another host. The Planner can edit only on `FINAL_INTEGRATION_AUTHORIZED`; the Reviewer denies both edit and shell access. For escalation, use a run-scoped Planner profile with the exact Reviewer-selected model and effort.

Official docs:
- https://opencode.ai/docs/skills
- https://opencode.ai/docs/agents
