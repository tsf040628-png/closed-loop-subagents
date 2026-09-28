# Codex Adapter

Install the repository as a skill-only plugin or install the directory `skills/closed-loop-subagents` as a Skill. A Skill package does not install or guarantee the runtime's subagent dispatcher; confirm that the selected Codex surface exposes independent subagent dispatch before starting a run.

The example profile uses gpt-6-luna / max for Planner, Executor, and Reviewer. The Reviewer may choose gpt-6-sol / medium for the Planner from corrective cycle 4 onward when it identifies a serious planning or route error.

The Controller must use the active runtime's native dispatch arguments and pass the exact role model and effort. Do not substitute a same-model parent or weaker model assignment without the user's approval. Record accepted assignment evidence separately from actual runtime telemetry. If the user's profile requires runtime telemetry and the host does not expose it, pause and ask whether assignment-only evidence is acceptable.

See https://developers.openai.com/plugins/build/plugins and the core references.
