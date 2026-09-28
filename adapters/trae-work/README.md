# TraeWork 适配说明

导入 Skill 时，将相关文件打包为 ZIP，并把 SKILL.md 放在压缩包根目录。配置模型前，询问用户正在使用 TraeWork 的哪种界面（网页、桌面或移动端）、哪种模式、哪个版本或发行版。

TraeWork 可用模型取决于当前模式。三个角色及规划代理升级模型均应从当前模型选择器或对应模式的官方模型列表中选择。不要把 Work 模式下选择的模型当作 Code 或 Design 模式的默认模型。

不要假设 TraeCode 的 Subagent 文件可以直接用于 TraeWork。目前公开文档介绍了 TraeCode 的 Subagents，而 TraeWork 企业功能表列出了 Skill 支持但未列出 Subagents。开始流程前，先确认当前 TraeWork 界面能否启动独立代理上下文。

如果不能独立派发，可提供 manual-review-bridge.md 中的手动桥接流程：由用户在单独上下文中运行审阅代理，再把审阅意见带回当前任务。此类审阅必须标记为 MANUAL_REVIEW，不能称为原生独立审阅。手动流程不是自动闭环；等待用户返回审阅结果，不要宣称流程已自动继续。

官方文档：
- Work Skills：https://docs.trae.cn/work_skills
- Work 模式：https://docs.trae.cn/work_design-system
- 模型选择：https://docs.trae.cn/work_models
- TraeWork 企业功能表：https://docs.trae.cn/enterprise_feature-list?lang=zh
- TraeCode Subagents（仅 TraeCode 文档）：https://docs.trae.cn/ide_subagents?lang=zh
