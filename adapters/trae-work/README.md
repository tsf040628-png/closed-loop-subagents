# TraeWork Adapter

For Skill import, package the Skill files with SKILL.md at the ZIP root. Ask the user which TraeWork surface (web, desktop, or mobile), mode, edition, and version they use before configuring models.

TraeWork model availability is mode-dependent. Populate each role and Planner-escalation model from the active model picker or documented model list. Never reuse a model selected for Work mode as an assumed Code or Design mode default.

Do not assume TraeCode's Subagent files work in TraeWork. Current public docs document Subagents for TraeCode, while the TraeWork enterprise feature matrix lists Skill support but does not list Subagents. Confirm the active TraeWork surface can launch a separate agent context before starting the workflow. If it cannot, offer the manual bridge in `manual-review-bridge.md`, requiring the user to run the Reviewer in a separate context and return its findings. Label that review `MANUAL_REVIEW`, not native independent review. Since this manual path is not an automatic closed loop, stop and wait for the user to return the review rather than claiming the workflow continued on its own.

Official docs:
- https://docs.trae.cn/work_skills
- https://docs.trae.cn/work_design-system
- https://docs.trae.cn/work_models
- https://docs.trae.cn/enterprise_feature-list?lang=zh
- https://docs.trae.cn/ide_subagents?lang=zh (TraeCode documentation only)
