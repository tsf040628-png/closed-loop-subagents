# Qoder 适配说明

通过 Qoder 的 Skill 发现或导入流程安装通用 Skill。本适配器中的原生代理文件仅面向 Qoder CLI。Qoder IDE/Quest 使用不同的扩展和代理打包格式；安装角色代理前，先确认用户当前使用的是哪种界面。

Qoder CLI 支持独立上下文、自定义子代理、按代理配置模型和 effort，以及有序编排。将 agents/ 下的文件复制到项目级目录 .qoder/agents/，或用户级目录 ~/.qoder/agents/；随后运行 /agents reload 并检查代理列表。只使用当前安装实际提供的模型标识和 effort 等级。模板默认使用 model: inherit；用户在首次配置中选择固定模型时，再替换该字段。省略 effort 表示继承当前会话设置。

规划代理升级模型应与三个角色的默认模型分开配置。独立审阅代理批准了符合条件的修正后，使用本次任务专用的规划代理覆盖设置，并传入预先配置的升级模型和 effort。Qoder CLI 支持通过 --agents 使用临时代理定义。派发前确认覆盖设置已被接受。规划代理只在明确收到 FINAL_INTEGRATION_AUTHORIZED 任务时拥有文件编辑授权；计划和重规划阶段不得编辑项目。审阅代理保持只读。

如果当前 Qoder 界面无法独立派发子代理或覆盖指定角色的模型，应记录该能力受限，并询问用户是否接受继承当前会话模型。

官方文档：
- Skills：https://docs.qoder.com/cli/Skills
- 内置代理：https://docs.qoder.com/cli/builtins-reference
- 子代理：https://docs.qoder.com/cli/subagent
- 模型：https://docs.qoder.com/cli/model
- IDE 子代理：https://docs.qoder.com/extensions/subagent
- Qoder Skills：https://docs.qoder.com/qoder/skills
