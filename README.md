# 闭环子代理

这是一个可配置、以审阅为关卡的多步骤协作流程。协调器通过版本化共享状态，安排规划代理、执行代理和独立审阅代理工作，并使用有上限的修正轮次和可核验证据判断是否完成。

Skill 主文件、参考文档和角色提示词使用英文；本 README 与面向使用者的平台说明使用中文。代理在配置提问和任务汇报时应使用用户的语言。

核心包遵循 [Agent Skills 规范](https://agentskills.io/specification)。平台能力说明依据厂商文档于 2026-09-28 核对；这不代表已在各产品中完成运行时兼容性认证。

## 工作流程

1. 规划代理在规划技术任务前查看相关 GitHub 方案，澄清影响方案的不确定事项，并提交带版本号的计划。
2. 独立审阅代理按验收标准检查计划；通过后才能进入执行，发现规划缺口时退回规划代理。
3. 执行代理每次只完成一个已批准步骤，随后由独立审阅代理检查产物和证据。
4. 审阅意见根据问题类型退回规划代理或执行代理。整个任务共享最多 8 轮修正预算。
5. 验收通过后，规划代理整合用户要求的最终文件并整理精简日志。清理前由审阅代理批准清理清单；规划代理只删除清单列出的临时日志，最后再进行一次闭环审查。

## 平台支持

Agent Skills 格式本身不规定子代理调度或模型选择 API。各平台适配说明将同一套角色约定映射到宿主，并明确平台无法提供的能力。

| 平台 | 支持级别 | 分发与代理能力 |
|---|---|---|
| Codex | 主要支持 | 可作为仅含 Skill 的插件打包，或单独安装 Skill。默认三个角色均使用 gpt-6-luna / max；从第 4 轮修正起，如审阅代理发现严重规划或路线错误，可要求规划代理升级到 gpt-6-sol / medium。 |
| Claude Code | 主要支持 | 使用原生 Skill 和自定义子代理。支持为不同代理设置模型与 effort；模板位于 adapters/claude-code/agents/。 |
| TraeWork | Skill 适配；编排能力需确认 | 可通过 ZIP 或 Skill 文件导入。公开文档介绍了 TraeCode 的 Subagents，但 TraeWork 功能表目前未列出该能力。应在实际 Work 客户端确认能否独立派发；否则只能使用标注清楚的手动桥接流程。 |
| Qoder | 主要支持 | 支持原生 Skills 与子代理。Qoder CLI 支持按代理设置模型与 effort；模板位于 adapters/qoder/agents/。不同界面能力可能不同。 |
| Cursor | 轻量桥接 | 使用原生 Skills 和自定义子代理。若账号方案、地区或团队策略不允许指定模型，Cursor 可能回退到其他模型。 |
| OpenCode | 轻量桥接 | 使用原生 Skills 和自定义代理，支持按代理配置模型。 |
| WorkBuddy | 轻量桥接 | 文档提供 Skill 包导入方式；是否支持独立派发和分角色模型，取决于具体产品形态与版本。 |

安装到某个平台前，请先阅读英文平台能力说明：[platform-adapters.md](skills/closed-loop-subagents/references/platform-adapters.md)。

## 首次配置

普通 Skill 文件无法在安装时弹出配置对话框。Agent Skills 规范定义了 Skill 文件及可选资源，没有下载时安装器回调，因此首次调用时会询问用户采用平台推荐配置还是自定义配置。配置内容包括：宿主平台与界面、三个角色的模型和推理设置、规划代理升级模型、模型核验策略、轮次上限、GitHub Star 门槛和日志详略。若平台支持，配置会保存到用户级位置；否则会提供配置文件和代理定义供用户保存。

Codex 推荐配置见 skills/closed-loop-subagents/assets/config.example.yaml。其他平台以当前会话模型作为建议起点，并使用该平台实际提供的模型 ID 保存角色设置；不会假定 Claude、TraeWork、Qoder、Cursor、OpenCode 或 WorkBuddy 的模型目录，也不会猜测模型标识。

## 安装

根据所用平台选择对应适配说明。若平台接受目录，可安装 skills/closed-loop-subagents 的内容；若要求上传压缩包，则创建 ZIP，并将 SKILL.md 放在压缩包根目录，同时包含其引用的文件。

导入 Skill 不会自动安装平台专用的代理定义。请将对应平台的模板复制到文档指定的代理目录，在首次配置时选择模型，并在运行前确认平台已发现这些定义。Codex 插件清单位于仓库根目录。

## 兼容性检查

计划中的平台检查项见 [compatibility-matrix.md](tests/compatibility-matrix.md)。发布时应分别记录 Skill 发现、独立子代理派发、分角色模型设置、运行时模型可观测性和配置持久化情况。仅能导入 Skill 并不代表整套流程都受支持。当前兼容性检查尚未在各厂商产品中执行。

## 上游致谢

本项目借鉴并改编了 obra/superpowers 中 subagent-driven-development Skill 的工作流理念及部分内容。详情见 [NOTICE.md](NOTICE.md) 和 [LICENSE](LICENSE)。

## 许可协议

本项目采用 MIT License。LICENSE 文件保留了上游版权与许可声明，并列明本项目新增的版权持有人。
