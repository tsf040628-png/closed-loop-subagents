# WorkBuddy 轻量桥接

通过 WorkBuddy 支持的 Skill 市场或本地 Skill 包导入功能安装。先确认用户指的是 WorkBuddy 桌面版、WorkBuddy 企业版、CodeBuddy 还是 Managed Agents；这些产品形态的安装方式和模型配置方式并不通用。

询问用户当前界面可用的角色模型和升级模型。声称审阅代理独立运行前，先核实当前界面是否支持独立子代理派发。如果只能导入 Skill，不能原生派发角色代理，请使用 [manual-role-bridge.md](manual-role-bridge.md)。此时由用户担任协调器，在不同上下文间传递带版本号的产物。每次审阅标记为 MANUAL_REVIEW；这是由用户操作的桥接流程，不是自动编排，也不代表宿主已核验原生代理派发。

官方文档：
- Skills：https://cloud.tencent.com/document/product/1831/134432
- Managed Agents：https://cloud.tencent.cn/document/api/1831/138581
