# Platform Adapters

The core Skill defines roles and review gates. An adapter maps them to host-native Skill discovery, agent files, dispatch tools, model identifiers, reasoning controls, persistent settings, and package import. Confirm the user's product surface and version because one vendor may expose different capabilities in its CLI, desktop, web, or enterprise editions.

## Support matrix

| Host | Skill entry | Independent-agent path | Model policy |
|---|---|---|---|
| Codex | Skill-only plugin or direct Skill directory | Native subagent dispatch | Use the Codex preset unless the user explicitly selects a supported override. Record accepted assignment separately from runtime telemetry. |
| Claude Code | SKILL.md under user or project Skills | Custom agents under user or project agent directories | Agent frontmatter supports model and effort. Ask which current model IDs to use. |
| TraeWork | Import a ZIP or skill file containing a root SKILL.md | Public docs currently document Subagents under TraeCode; the TraeWork feature matrix does not list them. Verify independent dispatch in the active Work surface. | Read model choices from the active mode. Do not reuse Work, Code, and Design models blindly. |
| Qoder | CLI user/project Skill folders or UI Skill import | Qoder CLI has built-in and custom agents; starter agent files are provided for CLI | Qoder CLI documents per-agent model and effort overrides. Confirm the CLI or UI surface before promising per-role settings. |
| Cursor | Native Skills or shared Skill locations | Custom agents in .cursor/agents; compatible agent directories are also recognized | The model field is supported, but the host can fall back for policy, subscription, or availability reasons. Starter frontmatter cannot deny nested Task dispatch per role; treat that boundary as prompt-only unless a verified host-level policy enforces it. |
| OpenCode | .opencode/skills or compatible .agents/skills | Native custom subagents | Per-agent model is configured with the host's provider/model identifier. |
| WorkBuddy | Skill marketplace or local Skill-package import | Depends on product surface and edition | Keep as a lightweight bridge until the exact runtime exposes and verifies per-agent dispatch and model controls. |

## Codex

Package the Skill in a plugin with a root plugin.json and a skills/ directory. The default role model is gpt-6-luna / max. The independent Reviewer may select gpt-6-sol / medium for a serious Planner error from corrective cycle 4 onward. Do not claim actual runtime model verification unless Codex exposes that telemetry for the dispatch.

Official packaging: https://developers.openai.com/plugins/build/plugins

## Claude Code

Claude Code loads Skills from user or project Skill directories and custom agents from user or project agent directories. Agent definitions support a model field and effort. Create one role definition for Planner, Executor, and Reviewer only after the user selects model settings; use inherit until then.

Official Skills: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview

Official subagents: https://code.claude.com/docs/en/sub-agents

## TraeWork

TraeWork Skill import supports a ZIP or skill file containing a root SKILL.md. Its public documentation lists Work, Code, and Design modes and host-managed model choices. Public docs describe Subagents for TraeCode; the TraeWork enterprise feature matrix does not list Subagents. Do not copy TraeCode agent paths into TraeWork by assumption. Confirm that the exact Work client can dispatch a separate agent context before treating its Reviewer as independent. If it cannot, offer a manual bridge that requires the user to run the Reviewer in a separate context, and label it `MANUAL_REVIEW`.

Skill installation and upload: https://docs.trae.cn/work_skills

Modes: https://docs.trae.cn/work_what-is-trae-work

Model selection: https://docs.trae.cn/work_models

TraeCode Subagent documentation (not proof of TraeWork support): https://docs.trae.cn/ide_subagents?lang=zh
TraeWork feature matrix: https://docs.trae.cn/enterprise_feature-list?lang=zh

## Qoder

Qoder supports Skills in user and project scopes. Qoder CLI documents independent built-in and custom subagents, per-agent model and effort settings, and ordered multi-agent orchestration. Use the CLI templates only in Qoder CLI; Qoder IDE/Quest uses a different extension and agent packaging path.

Skills: https://docs.qoder.com/cli/Skills

Subagents and model/effort fields: https://docs.qoder.com/cli/subagent
Built-in agents: https://docs.qoder.com/cli/builtins-reference
Qoder IDE custom agents: https://docs.qoder.com/extensions/subagent

## Cursor

Install the core Skill in a Cursor-discovered Skill directory. Use custom subagent definitions for the three roles. Model identifiers must be available to the current account and organization; Cursor documents compatible-model fallback. If the requested model cannot be enforced, report the fallback or assignment as unverified.

Skills: https://prod.cursor.com/docs/skills

Subagents and model configuration: https://prod.cursor.com/docs/subagents

## OpenCode

Use native .opencode/skills or its documented .agents/skills compatibility discovery. Define the roles as custom subagents and configure model identifiers using OpenCode's provider/model syntax. Keep settings in the user's or project's OpenCode configuration according to the requested scope.

Skills: https://opencode.ai/docs/skills

Agents: https://opencode.ai/docs/agents

## WorkBuddy

Use its Skill marketplace or import the platform's accepted Skill package. WorkBuddy surfaces differ between desktop, Enterprise, and Managed Agents. Do not assume that a desktop Skill package can configure a Managed Agent, or that a Skill importer exposes a generic install-time questionnaire. Verify subagent dispatch, per-agent models, and persistence on the user's exact surface.

Skills: https://cloud.tencent.com/document/product/1831/134432

Managed Agents: https://cloud.tencent.cn/document/api/1831/138581

## Capability fallback

For each run, record native Skills, independent subagents, per-role model assignment, runtime-model telemetry, and persistent configuration as AVAILABLE, LIMITED, UNAVAILABLE, or UNKNOWN.

- If independent dispatch is unavailable, do not claim an independent review. Stop or offer a manual, user-approved bridge.
- If role-specific model assignment is unavailable, ask whether inheriting the session model is acceptable. Record any degraded mode.
- If runtime telemetry is unavailable, report it honestly and audit only the accepted assignment.
- If persistent configuration is unavailable, show the user the resolved profile rather than storing it in a shared project.
