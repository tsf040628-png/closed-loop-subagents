# OpenCode 轻量桥接

将通用 Skill 安装到 .opencode/skills/closed-loop-subagents/，或放入文档说明的兼容目录。把 agents/ 中的三个代理模板复制到项目级目录 .opencode/agents/，或用户级目录 ~/.config/opencode/agents/。每个代理均使用 mode: subagent。

询问用户选择的服务提供方、模型 ID 和各角色的推理设置。若缺少 model 字段，子代理会继承发起它的主代理模型；首次配置时应将用户所选的 provider/model-id 和平台支持的 reasoningEffort 写入各代理。不要假定其他平台的模型 ID 或推理设置在 OpenCode 中同样有效。规划代理仅在收到 FINAL_INTEGRATION_AUTHORIZED 时才能编辑；审阅代理应禁用编辑和 Shell 权限。需要升级时，为本次任务单独配置规划代理，并使用审阅代理指定的模型和推理设置。

官方文档：
- Skills：https://opencode.ai/docs/skills
- 代理：https://opencode.ai/docs/agents
