# 实现细节（已被 Skill 取代）

> 本目录包含 `cn-skill-router` 和 `multica-cli-operator` 的参考实现 SKILL.md 文件。
> 这些文件是**具体实施细节**，供想了解 skill 内部逻辑的用户参考。

---

## 说明

以下 skill 已在实际 Multica workspace 中安装和运行：

| Skill | 用途 | 当前状态 |
|-------|------|---------|
| **cn-skill-router** | 中文任务路由和 skill 推荐 | 已在 workspace 安装运行 |
| **multica-cli-operator** | Multica CLI 安全操作 | 已在 workspace 安装运行 |
| **multica-workflow-bootstrapper** | 工作流初始化向导（入口） | 已在 workspace 安装运行 |

## 使用方式

如果你是新用户，**不需要手动复制本目录的文件**。推荐的使用流程是：

1. 加载 `multica-workflow-bootstrapper`（入口 skill）
2. 它会引导你完成需求访谈 → 配置推荐 → agent 指令生成
3. 如果需要创建 router/operator skill，bootstrapper 会引导你使用 `prompts/` 目录中的提示词

本目录的文件供以下场景使用：
- 想了解 skill 内部设计逻辑
- 想手动修改或定制 skill 内容
- 做离线参考

详细创建提示词见：
- `prompts/04-create-multica-cli-operator.zh.md`
- `prompts/05-create-cn-skill-router.zh.md`
- `prompts/08-create-workflow-bootstrapper.zh.md`
