# Qoder Adapter

Use Qoder's Skill discovery or import flow for the shared Skill. This adapter's native agent files target Qoder CLI only. Qoder IDE/Quest uses a separate extension and agent packaging format; ask which surface is active before installing role agents.

Qoder CLI supports separate contexts, custom subagents, per-agent model and effort settings, and ordered orchestration. Copy the files under `agents/` to `.qoder/agents/` (project scope) or `~/.qoder/agents/` (user scope), then run `/agents reload` and inspect the agents list. Use only model identifiers and effort levels currently available to that installation. The starter files use `model: inherit`; replace it with the model selected during first-use setup when the user chooses a pinned model. Omit `effort` to inherit the active session setting.

Keep Planner escalation separate from the three role defaults. When the independent Reviewer authorizes an eligible correction, use a run-scoped Planner override with the configured upgrade model and effort; Qoder CLI supports temporary agent definitions through `--agents`. Confirm the override was accepted before dispatch. The Planner retains write tools only for the explicit `FINAL_INTEGRATION_AUTHORIZED` step; its prompt forbids project edits during plan/replan calls. The Reviewer definition is read-only.

If the active Qoder surface cannot independently dispatch or override the requested role model, record the capability as limited and ask whether the user accepts session-model inheritance.

Official docs:
- https://docs.qoder.com/cli/Skills
- https://docs.qoder.com/cli/builtins-reference
- https://docs.qoder.com/cli/subagent
- https://docs.qoder.com/cli/model
- https://docs.qoder.com/extensions/subagent
- https://docs.qoder.com/qoder/skills
