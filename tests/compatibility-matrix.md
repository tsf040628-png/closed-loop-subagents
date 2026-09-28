# Release Compatibility Checks

These are release acceptance scenarios. They have not been executed against vendor applications.

For every host and surface, record the product version, date checked, Skill import path, subagent dispatch mechanism, model assignment fields, runtime telemetry availability, and configuration persistence behavior.

## Core scenarios

1. The Planner produces a plan and a distinct Reviewer receives it in an independent context.
2. A passing plan review permits execution; a plan gap routes to the Planner.
3. An execution defect routes to the Executor, followed by an independent re-review.
4. A routine next step keeps the configured models.
5. A serious planning or route error can trigger Reviewer-selected Planner escalation no earlier than corrective cycle 4.
6. Unsupported model assignment, rejected dispatch, or unconfirmed assignment is reported without silent substitution.
7. Missing runtime model telemetry is explicitly marked unobservable and never described as verified runtime identity.
8. If the saved profile requires runtime-model telemetry, an unobservable host pauses before dispatch and asks whether assignment-only evidence is acceptable.
9. At cycle 8, the Skill stops additional corrections and asks for explicit approval before a bounded extension.
10. Material ambiguity invokes grilling when available or asks frontier questions and waits for user confirmation.
11. Completion consolidates the final files and important log; cleanup requires a reviewed manifest and a separate closure review.
12. First-use setup is asked once, records host-specific models, and is not repeated when a valid profile exists.
13. GitHub research uses the configured star threshold, records sources, and does not clone or execute repositories without authorization.

## Host-specific notes

- Codex: verify the plugin/Skill loads and dispatch arguments accept the configured role models. Do not infer runtime telemetry from dispatch arguments.
- Claude Code: verify three custom agents load with the selected model and effort fields.
- TraeWork: verify the selected desktop/web/mobile surface, mode, edition, version, Skill import, and independent dispatch separately. Do not infer Work support from TraeCode Subagent documentation.
- Qoder: verify the selected CLI or UI surface. The included files target Qoder CLI; verify custom agent discovery and model/effort overrides there.
- Cursor: verify actual model assignment and detect documented fallback conditions.
- OpenCode: verify Skill discovery path and provider/model field handling.
- WorkBuddy: verify the exact desktop, Enterprise, or Managed Agents surface and whether role dispatch is independent.

Do not publish a host as fully supported until its core scenarios pass. If only import or prompt bridging works, label it as a bridge.
