# 安装指南

本指南记录各平台当前公开文档支持的安装方式。平台版本、权限和界面可能改变；当本机界面与文档有差异时，以该版本实际显示和对应厂商文档为准。这里的本地文件安装均要求安装完整技能目录。

## 1. 获取仓库

```bash
git clone https://github.com/tsf040628-png/closed-loop-subagents.git "$HOME/closed-loop-subagents"
```

也可以从[GitHub 仓库](https://github.com/tsf040628-png/closed-loop-subagents)选择 **Code → Download ZIP** 并解压。下文的 `$repo` 指解压后同时包含 `plugin.json`、`skills/` 和 `adapters/` 的仓库根目录；ZIP 解压目录可能带 `-main` 后缀。

## 2. 安装文件规则

- 无论平台如何，安装完整的 `skills/closed-loop-subagents/`：包括 `SKILL.md`、`references/` 和 `assets/`。不要只复制 `SKILL.md`。
- 原生角色文件只从仓库中对应的平台目录复制：Claude Code 用 `adapters/claude-code/agents/`，Qoder CLI 用 `adapters/qoder/agents/`，Cursor 用 `adapters/cursor/agents/`，OpenCode 用 `adapters/opencode/agents/`。模板中的模型默认继承会话或省略模型；按首次配置选择后再调整安装副本。
- 仓库没有 Codex、TraeWork 或 WorkBuddy 的原生角色模板。TraeWork 与 WorkBuddy 仅提供手动桥接文档，不能因安装了 Skill 就声称已配置原生角色代理。
- 用户级安装应用于该账户的多个项目；项目级安装只应用于当前仓库。将 `$repo`、路径中的 `$HOME` 和项目根目录替换成实际位置。
- 以下复制命令遇到已有 Skill 目标目录或同名代理文件时会停止，不会静默覆盖。更新已有安装前，先按下面的备份步骤移动旧 Skill 目录和冲突的代理文件到技能搜索目录之外的备份位置；检查备份后再重试。也可以选择一个尚未使用的项目级/用户级目标。

复制完整技能目录的通用命令如下。先按各平台章节选择 `$skillDest`，再执行对应系统的命令。

**Windows PowerShell**

```powershell
$repo = "C:\path\to\closed-loop-subagents"
$skillDest = Join-Path $HOME ".agents\skills\closed-loop-subagents" # 按平台章节替换
$source = Join-Path $repo "skills\closed-loop-subagents"
if (Test-Path -LiteralPath $skillDest) { throw "Skill 目标已存在。先备份或选择一个新目标：$skillDest" }
New-Item -ItemType Directory -Force -Path $skillDest | Out-Null
Copy-Item -Path "$source\*" -Destination $skillDest -Recurse
```

**macOS / Linux**

```bash
repo="$HOME/closed-loop-subagents"
skillDest="$HOME/.agents/skills/closed-loop-subagents" # 按平台章节替换
if [ -e "$skillDest" ]; then echo "Skill 目标已存在。先备份或选择一个新目标：$skillDest" >&2; exit 1; fi
mkdir -p "$skillDest"
cp -R "$repo/skills/closed-loop-subagents/." "$skillDest/"
```

需要安装角色模板的平台，再按平台章节设置 `$agentSource` 与 `$agentDest`，用下面的命令检查同名文件并复制。先检查所有目标名，再开始复制，避免发现冲突后只安装了一部分。

**Windows PowerShell**

```powershell
$agentFiles = Get-ChildItem -LiteralPath $agentSource -Filter "*.md" -File
$conflicts = @($agentFiles | Where-Object { Test-Path -LiteralPath (Join-Path $agentDest $_.Name) })
if ($conflicts.Count -gt 0) { throw "代理文件已存在。先备份冲突文件或选择新目标：$($conflicts.Name -join ', ')" }
New-Item -ItemType Directory -Force -Path $agentDest | Out-Null
Copy-Item -Path $agentFiles.FullName -Destination $agentDest
```

**macOS / Linux**

```bash
for sourceFile in "$agentSource"/*.md; do
  [ -e "$sourceFile" ] || continue
  target="$agentDest/$(basename "$sourceFile")"
  if [ -e "$target" ]; then echo "代理文件已存在。先备份冲突文件或选择新目标：$target" >&2; exit 1; fi
done
mkdir -p "$agentDest"
cp "$agentSource"/*.md "$agentDest/"
```

**备份已有内容后更新：**先把 `$skillDest` 设为本次平台章节中的完整 Skill 目标；若平台有角色模板，再设置对应的 `$agentSource` 和 `$agentDest`。Windows 下，没有代理模板的平台先运行 `$agentSource = $null; $agentDest = $null`。备份目录放在用户主目录下、平台技能/代理搜索路径之外。以下命令只移动旧 Skill 目录和与本次模板同名的代理文件，其他代理保留。备份后检查文件，再重新运行上述无覆盖复制命令。

Windows PowerShell：

```powershell
$backupName = ".closed-loop-subagents-backup-" + (Get-Date -Format "yyyyMMdd-HHmmss")
$backupRoot = Join-Path $HOME $backupName
if (Test-Path -LiteralPath $backupRoot) { throw "备份目录已存在，请换一个时间标记：$backupRoot" }
New-Item -ItemType Directory -Path $backupRoot | Out-Null
if (Test-Path -LiteralPath $skillDest) {
  Move-Item -LiteralPath $skillDest -Destination (Join-Path $backupRoot "skill")
}
if ($agentSource -and $agentDest -and (Test-Path -LiteralPath $agentSource) -and (Test-Path -LiteralPath $agentDest)) {
  $agentBackup = Join-Path $backupRoot "agents"
  $agentFiles = Get-ChildItem -LiteralPath $agentSource -Filter "*.md" -File
  $conflicts = @($agentFiles | Where-Object { Test-Path -LiteralPath (Join-Path $agentDest $_.Name) })
  if ($conflicts.Count -gt 0) {
    New-Item -ItemType Directory -Path $agentBackup | Out-Null
    foreach ($file in $conflicts) {
      Move-Item -LiteralPath (Join-Path $agentDest $file.Name) -Destination $agentBackup
    }
  }
}
```

macOS / Linux：

```bash
backupRoot="$HOME/.closed-loop-subagents-backup/$(date +%Y%m%d-%H%M%S)"
if [ -e "$backupRoot" ]; then echo "备份目录已存在，请换一个时间标记：$backupRoot" >&2; exit 1; fi
mkdir -p "$backupRoot"
if [ -e "$skillDest" ]; then mv "$skillDest" "$backupRoot/skill"; fi
if [ -n "${agentSource:-}" ] && [ -n "${agentDest:-}" ] && [ -d "$agentSource" ] && [ -d "$agentDest" ]; then
  for sourceFile in "$agentSource"/*.md; do
    [ -e "$sourceFile" ] || continue
    target="$agentDest/$(basename "$sourceFile")"
    if [ -e "$target" ]; then
      mkdir -p "$backupRoot/agents"
      mv "$target" "$backupRoot/agents/"
    fi
  done
fi
```

## 3. Codex

本节适用于 Codex CLI、IDE 扩展和支持本地 Skill 的 Codex 桌面环境。Codex 当前文档列出的用户级位置是 `$HOME/.agents/skills/`，仓库级位置是从工作目录到仓库根目录各层中的 `.agents/skills/`。`$HOME/.codex/skills/` 不是当前文档列出的默认路径。[Codex Skills 文档](https://developers.openai.com/codex/skills)

| 范围 | 目标目录 |
|---|---|
| 用户级 | `$HOME/.agents/skills/closed-loop-subagents/` |
| 项目级 | `<项目根目录>/.agents/skills/closed-loop-subagents/` |

按上一节设置 `$skillDest` 并复制。仓库根目录的 `plugin.json` 与 `skills/` 也符合便携插件的根目录布局；如选择插件分发，插件包根必须是仓库根目录，使 `plugin.json` 和 `skills/closed-loop-subagents/` 同处一层。插件目录 / marketplace 的启用方式请跟随[官方插件打包与分发说明](https://developers.openai.com/plugins/build/plugins)，本文不将某个未核验的 Codex 界面标签写成固定入口。

Codex 角色由宿主运行时的子代理派发机制提供；本仓库没有 `adapters/codex/agents/` 模板可供复制。新开会话；如技能列表未更新，重启 Codex。输入 `$` 并选择 `closed-loop-subagents`，或直接在请求中点名。Codex 文档说明可在 CLI / IDE 中用 `/skills` 查看或 `$` 调用技能；桌面环境的列表入口因界面而异。

本仓库 Codex 适配建议的初始角色设置为 `gpt-6-luna / max`，严重规划或路线错误符合条件时，Reviewer 可从第 4 个修正周期起提出将 Planner 改为 `gpt-6.1-sol / medium`。首次运行时仍须确认这些 ID 在当前 Codex 环境可选并取得用户确认；这只是 Codex 的建议，不是其他平台的模型目录。

**验证：**在技能列表中找到 `closed-loop-subagents`，并请求它只输出一个规划草案、不修改文件。确认 Skill 被调用。另行确认当前 Codex 运行环境确实提供独立子代理派发；Skill 安装本身不增加宿主没有的派发能力。

## 4. Claude Code

Claude Code 支持用户级和项目级 Skill 与自定义子代理。[Skills](https://code.claude.com/docs/en/skills) 和[子代理](https://code.claude.com/docs/en/sub-agents)文档列出的路径如下：

| 内容 | 用户级 | 项目级 |
|---|---|---|
| 完整 Skill | `~/.claude/skills/closed-loop-subagents/` | `<项目根目录>/.claude/skills/closed-loop-subagents/` |
| 三个角色文件 | `~/.claude/agents/` | `<项目根目录>/.claude/agents/` |

先把完整技能目录复制到对应目标。再把 `adapters/claude-code/agents/` 中三个 `.md` 文件复制到同一范围的 `agents/` 目录。例如 PowerShell 用户级安装：

```powershell
$repo = "C:\path\to\closed-loop-subagents"
$skillDest = Join-Path $HOME ".claude\skills\closed-loop-subagents"
$agentDest = Join-Path $HOME ".claude\agents"
$source = Join-Path $repo "skills\closed-loop-subagents"
if (Test-Path -LiteralPath $skillDest) { throw "Skill 目标已存在。先备份或选择新目标：$skillDest" }
$agentSource = Join-Path $repo "adapters\claude-code\agents"
$agentFiles = Get-ChildItem -LiteralPath $agentSource -Filter "*.md" -File
$conflicts = @($agentFiles | Where-Object { Test-Path -LiteralPath (Join-Path $agentDest $_.Name) })
if ($conflicts.Count -gt 0) { throw "代理文件已存在。先备份冲突文件或选择新目标：$($conflicts.Name -join ', ')" }
New-Item -ItemType Directory -Force -Path $skillDest, $agentDest | Out-Null
Copy-Item -Path "$source\*" -Destination $skillDest -Recurse
Copy-Item -Path $agentFiles.FullName -Destination $agentDest
```

macOS / Linux 用户级路径对应 `~/.claude/skills/closed-loop-subagents/` 与 `~/.claude/agents/`。项目级安装时改用项目根目录下的 `.claude/skills/closed-loop-subagents/` 和 `.claude/agents/`。

新建 `agents/` 目录或首次创建该目录中的代理后，重启 Claude Code，再运行 `/skills` 检查 Skill。用一个无副作用请求点名 `closed-loop-planner`；在 Claude Code 执行记录中确认出现了该子代理委派。已存在并被监视的目录后续改动通常可自动加载；若首次创建 Skill 根目录，可运行 `/reload-skills`。具体行为受 Claude Code 版本影响，见上述官方文档。

## 5. TraeWork

TraeWork 的官方上传流程是：左侧导航栏顶部 **插件市场 → 技能 → 上传技能**，选择本地 ZIP 或 `.skill` 文件并确认；导入后技能列在“已安装”中，默认启用。技能包必须在压缩包根目录直接包含 `SKILL.md`。本仓库的 `references/`、`assets/` 要作为 `SKILL.md` 的同级目录一并放入包中。[TraeWork 技能文档](https://docs.trae.cn/work_skills)

**Windows PowerShell 生成 ZIP：**

```powershell
$repo = "C:\path\to\closed-loop-subagents"
$source = Join-Path $repo "skills\closed-loop-subagents"
$zip = Join-Path $repo "closed-loop-subagents-skill.zip"
if (Test-Path -LiteralPath $zip) { throw "ZIP 已存在，请换一个文件名或先备份：$zip" }
Compress-Archive -Path "$source\*" -DestinationPath $zip
```

**macOS / Linux 生成 ZIP：**

```bash
repo="$HOME/closed-loop-subagents"
zip="$repo/closed-loop-subagents-skill.zip"
if [ -e "$zip" ]; then echo "ZIP 已存在，请换一个文件名或先备份：$zip" >&2; exit 1; fi
cd "$repo/skills/closed-loop-subagents" || { echo "找不到技能目录：$repo/skills/closed-loop-subagents" >&2; exit 1; }
zip -r "$zip" .
```

上传前检查 ZIP 最外层直接能看到 `SKILL.md`、`references/` 和 `assets/`；不能先进入一个额外的 `closed-loop-subagents/` 子目录才看到 `SKILL.md`。导入后开一个新任务，在输入框键入 `/` 选技能，或明确要求调用 `closed-loop-subagents`。

本仓库没有 TraeWork 原生代理模板。公开文档将 Subagent 说明放在 TraeCode 产品文档中，企业功能表也将其列在 TraeCode 功能项下；这不能证明任一 TraeWork 版本或 Work / Code / Design 模式都支持相同的独立派发。请在实际界面确认独立代理上下文与角色权限。如果没有，按 [`adapters/trae-work/manual-review-bridge.md`](adapters/trae-work/manual-review-bridge.md) 让用户在另一个上下文手动审阅，并标注 `MANUAL_REVIEW`；不要将它描述成自动闭环。[TraeCode Subagent 文档](https://docs.trae.cn/ide_subagents?lang=zh) · [Trae 企业功能清单](https://docs.trae.cn/enterprise_feature-list?lang=zh)

## 6. Qoder CLI

以下路径和角色文件只针对 **Qoder CLI**。Qoder CLI 官方文档列出用户级 `~/.qoder/skills/`、项目级 `.qoder/skills/`，代理目录为 `~/.qoder/agents/` / `.qoder/agents/`；不要把本仓库的 CLI 代理模板自动当作 Qoder IDE / Quest 的已验证安装包。Qoder IDE 有自己的技能导入和自定义代理文档，使用时先按对应界面确认模板格式。[Qoder CLI Skills](https://docs.qoder.com/cli/Skills) · [Qoder CLI Subagent](https://docs.qoder.com/cli/subagent) · [Qoder IDE Skills](https://docs.qoder.com/qoder/skills) · [Qoder IDE Custom Agent](https://docs.qoder.com/extensions/subagent)

| 内容 | 用户级 | 项目级 |
|---|---|---|
| 完整 Skill | `~/.qoder/skills/closed-loop-subagents/` | `<项目根目录>/.qoder/skills/closed-loop-subagents/` |
| 三个角色文件 | `~/.qoder/agents/` | `<项目根目录>/.qoder/agents/` |

复制完整 Skill，再把 `adapters/qoder/agents/` 下的三个 `.md` 文件复制到相同安装范围的 `agents/` 目录。新建目录示例：

```powershell
$repo = "C:\path\to\closed-loop-subagents"
$skillDest = Join-Path $HOME ".qoder\skills\closed-loop-subagents"
$agentDest = Join-Path $HOME ".qoder\agents"
$source = Join-Path $repo "skills\closed-loop-subagents"
$agentSource = Join-Path $repo "adapters\qoder\agents"
if (Test-Path -LiteralPath $skillDest) { throw "Skill 目标已存在。先备份或选择新目标：$skillDest" }
$agentFiles = Get-ChildItem -LiteralPath $agentSource -Filter "*.md" -File
$conflicts = @($agentFiles | Where-Object { Test-Path -LiteralPath (Join-Path $agentDest $_.Name) })
if ($conflicts.Count -gt 0) { throw "代理文件已存在。先备份冲突文件或选择新目标：$($conflicts.Name -join ', ')" }
New-Item -ItemType Directory -Force -Path $skillDest, $agentDest | Out-Null
Copy-Item -Path "$source\*" -Destination $skillDest -Recurse
Copy-Item -Path $agentFiles.FullName -Destination $agentDest
```

macOS / Linux 用户级目录分别为 `~/.qoder/skills/closed-loop-subagents/` 和 `~/.qoder/agents/`；项目级目录分别为 `.qoder/skills/closed-loop-subagents/` 和 `.qoder/agents/`。

如 CLI 已运行，执行 `/skills reload` 与 `/agents reload`；用 `/skills` 和 `/agents` 检查资源。也可用 `qoder agents list` 查看代理。再用一条只读小请求点名 `closed-loop-planner`，确认委派记录出现。Qoder CLI 文档称角色可配置模型 / effort；仅从当前 CLI 的可用值中选取，不要照搬 Codex 模型名。

## 7. Cursor

Cursor 支持用户级与项目级技能目录，也会发现自定义子代理文件。[Cursor Skills](https://prod.cursor.com/docs/skills) · [Cursor Subagents](https://prod.cursor.com/docs/subagents)

| 内容 | 用户级 | 项目级 |
|---|---|---|
| 完整 Skill | `~/.cursor/skills/closed-loop-subagents/` | `<项目根目录>/.cursor/skills/closed-loop-subagents/` |
| 三个角色文件 | `~/.cursor/agents/` | `<项目根目录>/.cursor/agents/` |

也可用文档支持的 `.agents/skills/` 兼容技能目录，但本指南使用 Cursor 原生 `.cursor/` 位置。复制完整 Skill，再把 `adapters/cursor/agents/` 中三个 `.md` 文件复制到相应 `agents/` 目录；角色文件按第 2 节的无覆盖复制命令操作。新开 Agent 会话后，在 Agent 输入框键入 `/` 查找 Skill；点名 `closed-loop-planner` 并确认实际委派记录，作为角色安装检查。

Cursor 子代理模板可以设置 `model` 字段，但账号套餐、组织策略或模型可用性会导致宿主回退；请先询问用户当前可用的模型 ID 和角色偏好，确认实际会话/子代理报告后再记为已核验。模型字段或用户选择只证明配置/分配，不自动证明运行时实际模型。[Cursor 子代理模型说明](https://prod.cursor.com/docs/subagents)

## 8. OpenCode

OpenCode 原生技能和代理目录如下：[Skills](https://opencode.ai/docs/skills) · [Agents](https://opencode.ai/docs/agents)

| 内容 | 用户级 | 项目级 |
|---|---|---|
| 完整 Skill | `~/.config/opencode/skills/closed-loop-subagents/` | `<项目根目录>/.opencode/skills/closed-loop-subagents/` |
| 三个角色文件 | `~/.config/opencode/agents/` | `<项目根目录>/.opencode/agents/` |

OpenCode 也会发现 `~/.agents/skills/` / `.agents/skills/` 及 Claude 兼容路径；这里优先用 OpenCode 原生目录。复制完整技能目录，再将 `adapters/opencode/agents/` 下的三个 `.md` 文件复制到相同范围的 `agents/` 目录；角色文件按第 2 节的无覆盖复制命令操作。模板设为 `mode: subagent`，没有预设跨平台模型 ID；首次配置后再为角色选择 OpenCode 接受的 `provider/model` 标识。

复制后重新打开 OpenCode 会话，在任务中确认技能出现在可用技能列表；再明确要求调用某个 `closed-loop-*` 子代理，确认产生独立子代理上下文。没有被宿主显示的模型遥测时，只记录已接受的配置，并标注 `RUNTIME_MODEL_UNOBSERVABLE`。

## 9. WorkBuddy

本节的可核验入口限于腾讯文档所述 **WorkBuddy Enterprise 本地 AI 工作台 / 技能界面**，不代表 CodeBuddy CLI、Managed Agents 或其他 WorkBuddy 发行形态都使用相同界面。[WorkBuddy Enterprise 技能](https://cloud.tencent.com/document/product/1831/134432)说明可通过左侧边栏的 **专家·技能·连接器** 打开技能管理，选择添加技能并上传本地技能包；导入后在已安装技能中启用，并在任务对话中选择或召唤技能。[本地 AI 工作台](https://cloud.tencent.com/document/product/1831/134391)

仓库提供的待上传资源是完整的 `skills/closed-loop-subagents/` 文件夹，需保留 `SKILL.md`、`references/` 与 `assets/`。目前查到的 WorkBuddy 官方技能上传文档没有说明 ZIP 内部必须采用哪种根层布局，因此不能承诺 TraeWork 的“ZIP 根直接是 `SKILL.md`”要求也适用于 WorkBuddy。若界面接受 ZIP，可先试标准单技能目录包：包内一层 `closed-loop-subagents/`，其下放完整技能目录内容；如果客户端提示了明确布局，则按提示为当前版本调整。

仓库中 `adapters/workbuddy/` 只有手动角色桥接说明，没有 WorkBuddy 原生代理模板。腾讯的子代理文档属于 **CodeBuddy Code** 运行环境，不能据此宣称 WorkBuddy 本地 AI 工作台支持相同的独立调度。若当前 WorkBuddy 界面没有可验证的独立角色派发，使用 [`adapters/workbuddy/manual-role-bridge.md`](adapters/workbuddy/manual-role-bridge.md)，由用户把材料交给独立上下文审阅，并标注 `MANUAL_REVIEW`；它不是自动闭环。[CodeBuddy Code 子代理文档](https://cloud.tencent.com/document/product/1831/137015)

可选 ZIP 生成示例（文档没有说明包内根层，所以该结构只是标准目录打包建议，不保证所有 WorkBuddy 版本都接受）：

```powershell
$repo = "C:\path\to\closed-loop-subagents"
$skill = Join-Path $repo "skills\closed-loop-subagents"
$zip = Join-Path $repo "closed-loop-subagents-workbuddy.zip"
if (Test-Path -LiteralPath $zip) { throw "ZIP 已存在，请换一个文件名或先备份：$zip" }
Compress-Archive -Path $skill -DestinationPath $zip
```

该命令把 `closed-loop-subagents/` 作为 ZIP 顶层目录，目录内保留 `SKILL.md`、`references/` 和 `assets/`。如上传器要求不同的根层布局，按当前版本提示调整；不要把此处假设写成厂商保证。

## 10. 首次使用：确认平台、模型和角色偏好

安装不会弹出通用的安装后配置对话框。第一次明确调用 `closed-loop-subagents` 时，先确认以下信息再派发：

1. 产品、运行界面、版本和模式（例如 Qoder CLI 与 Qoder IDE、TraeWork 与 TraeCode、WorkBuddy 与 CodeBuddy CLI 分开确认）。
2. 该宿主当前确实显示的可用模型标识、推理强度 / effort（若支持），以及 Planner、Executor、Reviewer 各自偏好的设置。
3. 是否配置单独的 Planner 升级模型及触发条件；不支持独立配置时，询问用户是否接受继承会话模型。
4. 当前宿主是否能独立派发这些角色、是否能显示每个角色实际运行的模型、配置是否持久化。

除了 Codex 的平台专用建议外，不得预先给其他平台填入 Codex 模型 ID 或自行套用其他厂商的模型目录。Codex 的配置建议也要向用户确认后才启用。其他平台的代理模板默认继承会话模型或不写模型；只有用户从当前宿主可见选项中确认后，才修改用户级安装副本中的模型字段。若宿主无法按角色设置模型，先询问用户是否接受继承；若用户要求运行时模型核验但宿主不显示该遥测，则暂停派发并说明限制。

模型分配和运行时模型是两种证据。记录宿主接受的模型设置只能说明“已分配”；只有实际运行面板或记录明确显示模型时才写“已核验”。否则标注 `RUNTIME_MODEL_UNOBSERVABLE`。若独立审阅只能由用户在另一个上下文启动，标注 `MANUAL_REVIEW`，不要称为原生自动审阅。

## 11. 安装后快速检查

- 文件安装：目标目录中同时存在 `SKILL.md`、`references/`、`assets/`；模板平台的 `agents/` 目录中存在三个角色文件。
- UI 导入：技能出现在“已安装”或平台的技能列表中，并处于启用状态。
- 开一个低风险的新会话，明确调用 `closed-loop-subagents`，请求只生成不修改文件的简短计划。
- 对原生代理平台，点名 Planner 并检查宿主是否显示独立委派；只看到 Skill 回复不等于验证了子代理调度。
- 如果平台不支持原生独立委派，停止原生闭环并在用户同意后使用手动桥接；审阅结果保持 `MANUAL_REVIEW` 标签。
- 查看角色模型配置是否与用户确认的本机选项一致。没有运行时遥测时如实标为 `RUNTIME_MODEL_UNOBSERVABLE`。

## 12. 本指南使用的官方文档

- OpenAI：[Codex Skills](https://developers.openai.com/codex/skills)、[插件打包](https://developers.openai.com/plugins/build/plugins)
- Anthropic：[Claude Code Skills](https://code.claude.com/docs/en/skills)、[Claude Code 子代理](https://code.claude.com/docs/en/sub-agents)
- TRAE：[TraeWork 技能](https://docs.trae.cn/work_skills)、[TraeCode Subagent](https://docs.trae.cn/ide_subagents?lang=zh)、[Trae 企业功能清单](https://docs.trae.cn/enterprise_feature-list?lang=zh)
- Qoder：[CLI Skills](https://docs.qoder.com/cli/Skills)、[CLI Subagent](https://docs.qoder.com/cli/subagent)、[IDE Skills](https://docs.qoder.com/qoder/skills)、[IDE Custom Agent](https://docs.qoder.com/extensions/subagent)
- Cursor：[Skills](https://prod.cursor.com/docs/skills)、[Subagents](https://prod.cursor.com/docs/subagents)
- OpenCode：[Skills](https://opencode.ai/docs/skills)、[Agents](https://opencode.ai/docs/agents)
- 腾讯 WorkBuddy：[WorkBuddy Enterprise 技能](https://cloud.tencent.com/document/product/1831/134432)、[本地 AI 工作台](https://cloud.tencent.com/document/product/1831/134391)、[CodeBuddy Code 子代理](https://cloud.tencent.com/document/product/1831/137015)
