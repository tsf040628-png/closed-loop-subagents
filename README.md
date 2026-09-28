# Closed-Loop Subagents

A configurable, review-gated workflow for multi-step work. A Controller coordinates a Planner, an Executor, and an independent Reviewer through versioned shared state, bounded correction cycles, and evidence-based completion.

The Skill instructions are written in English. The agent should use the user's language for setup questions and task reports.

The core package follows the [Agent Skills specification](https://agentskills.io/specification). Platform capability notes were checked against vendor documentation on 2026-09-28; they are not runtime compatibility certifications.

## Workflow

1. The Planner checks relevant GitHub prior art before planning technical work, clarifies material uncertainty, and writes a versioned plan.
2. An independent Reviewer approves or returns the plan with findings.
3. The Executor completes one approved step at a time. An independent Reviewer checks each result and its evidence.
4. Reviewer findings route to the Planner or Executor. Corrections share one run-wide budget of eight cycles.
5. After acceptance passes, the Planner integrates requested files and proposes a compact log. A separate Reviewer approves the cleanup manifest before the Planner removes only listed scratch logs. A closure review verifies the final state.

## Host support

The shared Skill format does not define subagent dispatch or model-selection APIs. Platform adapters map the same role contracts to each host and disclose capabilities that the host cannot provide.

| Host | Support level | Distribution and agent support |
|---|---|---|
| Codex | Primary | Packaged as a skill-only plugin or installed as a Skill. The default profile is gpt-6-luna / max for all roles; the Reviewer may request gpt-6-sol / medium for the Planner from correction cycle 4 when it finds a serious planning or route error. |
| Claude Code | Primary | Native Skill plus custom subagents. Per-agent model and effort fields are available; starter agent definitions are in `adapters/claude-code/agents/`. |
| TraeWork | Primary Skill adapter; orchestration capability-gated | Native Skill import through a ZIP or skill file. Public docs list Subagents under TraeCode, while TraeWork's feature matrix does not currently list them. Confirm independent dispatch in the exact Work surface; otherwise use only the labeled manual bridge. |
| Qoder | Primary | Native Skills and subagents. Qoder CLI supports per-agent model and effort settings; starter definitions are in `adapters/qoder/agents/`. UI capabilities differ. |
| Cursor | Lightweight bridge | Uses native Skills and custom subagents. Starter definitions are included. A requested model can fall back when plan, region, or team policy prevents it. |
| OpenCode | Lightweight bridge | Uses native Skills and custom agents with per-agent model configuration. Starter definitions are included. |
| WorkBuddy | Lightweight bridge | Skill-package import is documented. Per-agent dispatch and model controls depend on the exact surface and edition. |

Read skills/closed-loop-subagents/references/platform-adapters.md before installing on a host. It links the relevant official documentation and records the current compatibility boundary.

## First-use configuration

A plain Skill file cannot run an installation-time dialog. The Agent Skills format defines Skill files and optional resources, not a download-time installer callback, so the first Skill invocation asks once whether to use the host's recommended profile or customize it. Setup collects the host and surface, each role model and reasoning setting, Planner escalation model, model-verification policy, cycle cap, GitHub star threshold, and log detail. It stores settings in a host-appropriate user-level configuration location when possible; otherwise it gives the user a config file and configured agent definitions to save.

The Codex preset is provided in `skills/closed-loop-subagents/assets/config.example.yaml`. Other host profiles use that platform's current session model as the recommended starting point, then save any role-specific choices using that host's native model IDs. They deliberately do not assume a Claude, TraeWork, Qoder, Cursor, OpenCode, or WorkBuddy model catalog. Model identifiers are never guessed.

## Installation

Use the adapter for the selected host. For hosts that accept a directory, install the contents of `skills/closed-loop-subagents`. For hosts that require an uploaded package, create a ZIP with `SKILL.md` at the archive root and include its referenced files. A Skill import does not automatically install platform-specific agent definitions: copy the selected host's starter files into the documented agent directory, choose models during first-use setup, and verify discovery before a run. The Codex plugin manifest is at the repository root.

## Compatibility checks

The planned host checks are listed in tests/compatibility-matrix.md. A release should report Skill discovery, independent subagent dispatch, per-role model assignment, runtime-model observability, and setup persistence separately. Skill import alone does not establish full workflow support.

## Attribution

This project adapts workflow ideas and portions of the subagent-driven-development Skill in obra/superpowers. See NOTICE.md and LICENSE. Superpowers is distributed under the MIT License.

## License

This project is licensed under the MIT License. The LICENSE file preserves the upstream copyright notice and identifies the additional copyright holder for this adaptation.
