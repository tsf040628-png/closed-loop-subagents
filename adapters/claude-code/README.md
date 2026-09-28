# Claude Code Adapter

Install the shared Skill in Claude Code's user or project Skill directory. Copy the three starter definitions from `agents/` to `.claude/agents/` (project scope) or `~/.claude/agents/` (user scope). Keep Planner, Executor, and Reviewer as separate custom agents so review has an isolated context.

During first-use setup, ask the user for each role's model and effort. The templates start with `model: inherit`; omitted effort inherits the session setting. Replace the model fields and add an `effort` field only if the user selects an explicit, supported effort. Claude Code can accept a per-invocation model, which the Controller can use for Reviewer-approved Planner escalation from cycle 4. Check `/tasks` while the agent runs when runtime model confirmation is needed; record only what the host actually reports.

The Planner agent has write-capable tools because the final workflow requires it to integrate deliverables. Its prompt only authorizes file edits during the explicit `FINAL_INTEGRATION_AUTHORIZED` dispatch. The Reviewer definition has a read-only tool allowlist. Keep role definitions separate from the core Skill so model changes do not fork the workflow.

Do not claim actual runtime-model verification unless the current Claude Code surface exposes it.

Official docs:
- https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- https://code.claude.com/docs/en/sub-agents
