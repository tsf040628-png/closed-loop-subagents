# 闭环子代理

一个由独立审阅把关的多步骤协作 Skill：先审计划，再按批准步骤逐项执行；每项工作都要提交产物和证据，经 Reviewer 通过后才能继续。面向使用者的 README、安装指南和平台说明使用中文；核心 Skill、参考文档和角色模板保留英文。

## 功能一览

| 环节 | 功能 |
|---|---|
| 规划 | Planner 拆解目标并定义验收标准；技术任务可先查找 GitHub 参考方案，关键不确定点及时向你确认。 |
| 执行与审阅 | Reviewer 先审计划，再独立复核 Executor 按批准步骤提交的产物和证据。 |
| 修正 | 发现退回对应角色处理，共用最多 8 轮预算；达到上限仍未完成时汇报并请求授权增加轮次。 |
| 收尾 | Planner 整合产物并提出日志清理清单，经 Reviewer 复核后完成清理和闭环检查。 |
| 状态记录 | 共享状态保存计划、产物、证据、审阅结论和模型分配信息。 |

## 角色关系

点击图表可打开交互版。

[![闭环子代理架构图](docs/diagrams/role-architecture.svg)](https://tsf040628-png.github.io/closed-loop-subagents/?diagram=architecture)



## 工作流与修正路由



[![闭环子代理工作流图](docs/diagrams/workflow.svg)](https://tsf040628-png.github.io/closed-loop-subagents/?diagram=workflow)







## 平台支持

Skill 文件格式本身不保证宿主提供独立子代理调度、分角色模型设置或运行时模型遥测。下表概述仓库适配范围；请按平台、版本和当前界面确认实际能力。

| 平台 | 支持方式与边界 |
|---|---|
| Codex | 原生 Skill 和宿主子代理调度；本仓库不提供 Codex 角色模板。默认角色配置为 `gpt-6-luna / max`；符合条件时 Reviewer 可决定从第 4 个修正周期起将 Planner 指派为 `gpt-6-sol / medium`。若宿主不提供运行时遥测，只能报告模型分配，不能声称核验了实际运行模型。 |
| Claude Code | 原生 Skill 和自定义子代理；角色模板支持模型与 effort 字段。先确认当前版本提供的模型 ID 和设置。 |
| TraeWork | 支持 Skill 包导入；公开资料未证明当前 Work 界面具备独立子代理调度。需在实际界面确认；不支持时可使用手动审阅桥接，并标记 `MANUAL_REVIEW`。 |
| Qoder CLI | CLI 支持 Skill、自定义子代理及每代理模型 / effort 配置；仓库模板面向 Qoder CLI，不能据此推断 Qoder IDE 或 Quest 使用相同路径和能力。 |
| Cursor | 原生 Skill 和自定义子代理。账号、订阅或组织策略可能导致模型回退；模板提示不能代替宿主级权限限制，需确认 Reviewer 上下文确实独立。 |
| OpenCode | 原生 Skill 和自定义子代理；模型需按当前服务商使用 OpenCode 的 `provider/model` 标识。 |
| WorkBuddy | 支持技能市场或本地技能包导入；是否提供独立子代理、角色模型设置和持久化取决于产品形态与版本。不能独立审阅时，使用手动桥接并标记 `MANUAL_REVIEW`。 |

平台支持说明是适配指引，不代表已完成所有宿主版本的运行时兼容认证。

## 安装

先克隆仓库，或从 GitHub 下载 ZIP；然后按平台安装完整的 `skills/closed-loop-subagents/` 目录。仓库附带的原生角色模板仅位于 `adapters/claude-code/agents/`、`adapters/qoder/agents/`（Qoder CLI）、`adapters/cursor/agents/` 和 `adapters/opencode/agents/`。Codex 使用宿主原生子代理调度，本仓库没有 Codex 角色模板；TraeWork 和 WorkBuddy 对应手动桥接文件，本仓库不提供这两者的原生 agent 模板。

```bash
git clone https://github.com/tsf040628-png/closed-loop-subagents.git
```

不要只复制 `SKILL.md`：`references/` 和 `assets/` 中包含所需规则、平台说明和配置样例。逐个平台的安装路径、导入方式和安装后检查见[安装指南](INSTALLATION.md)。首次使用时确认当前平台界面、可用模型和设置；不要把 Codex 的模型标识当作其他平台的配置，也不要把模型分配当作实际运行时遥测。

## 项目文件

- 核心 Skill：`skills/closed-loop-subagents/`
- 平台能力说明：`skills/closed-loop-subagents/references/platform-adapters.md`
- Claude Code 角色模板：`adapters/claude-code/agents/`
- Qoder CLI 角色模板：`adapters/qoder/agents/`
- Cursor 角色模板：`adapters/cursor/agents/`
- OpenCode 角色模板：`adapters/opencode/agents/`
- TraeWork 手动审阅桥接：`adapters/trae-work/manual-review-bridge.md`
- WorkBuddy 手动角色桥接：`adapters/workbuddy/manual-role-bridge.md`
- 安装指南：[INSTALLATION.md](INSTALLATION.md)
- 兼容性检查表：[tests/compatibility-matrix.md](tests/compatibility-matrix.md)

## 上游致谢与许可

本项目参考了 [obra/superpowers 的 subagent-driven-development Skill](https://github.com/obra/superpowers/tree/main/skills/subagent-driven-development) 的流程展示方式，也参考了 [anthropics/skills](https://github.com/anthropics/skills) 的用户导向文档组织方式；本项目没有因此复制上游代码。详情见 [NOTICE.md](NOTICE.md)。本项目使用 MIT License，见 [LICENSE](LICENSE)。


