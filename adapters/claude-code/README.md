# Claude Code 适配说明

将通用 Skill 安装到 Claude Code 的用户级或项目级 Skill 目录。把 agents/ 下的三个代理模板复制到项目级目录 .claude/agents/，或用户级目录 ~/.claude/agents/。规划、执行和审阅应保持为三个独立自定义代理，使审阅拥有独立上下文。

首次配置时，询问用户为每个角色选择的模型和 effort。模板默认使用 model: inherit；未填写 effort 时沿用会话设置。只有用户明确选择了平台支持的固定模型或 effort 后，才替换模板字段。Claude Code 支持按次调用指定模型，协调器可在第 4 轮起按审阅代理批准的要求升级规划代理。需要确认运行时模型时，查看代理运行期间的 /tasks 信息；只记录宿主实际显示的内容。

规划代理需要写入工具，以便在流程末尾整合交付物。其提示词只在明确收到 FINAL_INTEGRATION_AUTHORIZED 派发时授权修改文件。审阅代理模板配置为只读工具白名单。请将角色定义与 Skill 主文件分开维护，避免因调整模型设置而分叉工作流。

只有当前 Claude Code 界面能提供运行时模型信息时，才报告实际运行模型已核验。

官方文档：
- Skill：https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview
- 子代理：https://code.claude.com/docs/en/sub-agents
