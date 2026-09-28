# First-Run Setup

## When to ask

A Skill download or import normally copies files; it does not start the Skill or expose a shared installer callback. Ask for configuration on first activation unless the selected host provides a supported installation-time setup hook. Do not repeat setup when a valid profile already exists. Let the user reopen setup when they ask to change preferences.

## Setup sequence

1. Identify the host, product surface, version, and mode. If host detection is uncertain, ask the user.
2. Read the matching adapter. Check whether it supports native Skills, independent subagents, per-role model assignment, model-effort controls, and persistent user configuration.
3. Present one compact setup card with a recommended choice and a customize choice.
4. Resolve the available model names from the host UI or documented host configuration. If they are not visible, ask the user to provide the exact identifiers. Never invent them.
5. Ask for the Planner, Executor, Reviewer, and Planner-escalation model settings. Codex may use the published preset. Other hosts default to the active session model until the user chooses role-specific assignments.
6. Confirm the loop cap, GitHub star threshold, clarification behavior, runtime-model verification policy, log detail, and output language. Offer the documented defaults: eight correction cycles, 1,000 stars for relevant GitHub prior art, grilling when available, report-only handling when runtime telemetry is unavailable, compact logs, and the user's language. If the user requires runtime-model telemetry, only proceed on a host that exposes it.
7. Save the resolved profile in a user-level location supported by the host. If writing a persistent setting is unsupported or not authorized, show the exact config content and let the user keep it locally. Do not write personal settings into a public repository by default.
8. Where native agent definitions are supported, prepare the Planner, Executor, and Reviewer files using the resolved host-specific models and permissions. Ask for the desired scope (project or user) before writing outside the current project. If the host requires manual installation, provide the completed files and exact destination paths.

## Prompt

Use this prompt in the user's language:

> I need to configure Closed-Loop Subagents for this host. I found these capabilities: [host, surface, subagent support, model controls, telemetry, persistence]. Would you like to use the recommended profile or customize it? The recommended workflow uses a shared eight-cycle cap, searches relevant GitHub prior art from 1,000 stars, asks about material ambiguity, and keeps a compact evidence-backed log. Please confirm the model for Planner, Executor, Reviewer, and Planner escalation from the models available here. If this host cannot assign a separate model to a role, I will tell you and ask whether session-model inheritance is acceptable.

If the user chooses the recommended profile, save it without asking the same questions again. If a material choice remains unresolved, ask only that choice and do not dispatch agents until the model policy is resolved.
