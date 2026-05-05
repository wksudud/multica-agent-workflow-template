# 05 — 创建 cn-skill-router Skill

> 将此提示词发送给你的 AI agent，创建中文任务路由 skill。

---

请帮我创建一个新的 skill：`cn-skill-router`

## Skill 用途

中文任务路由器，用于：
- 帮中文用户理解已安装的 skills
- 根据中文任务判断任务类型和复杂度
- 推荐最合适的 Multica agent
- 推荐 1-3 个最合适的 skill
- 给出任务派单建议和 issue 模板

## 重要定位

1. cn-skill-router 不直接操作 Multica CLI
2. 实际创建 issue 和分配 agent 使用 multica-cli-operator
3. cn-skill-router 是导航层（判断 + 推荐），multica-cli-operator 是执行层

## 内容要求

1. YAML frontmatter（name + description）
2. 两种模式：维护模式（扫描 skills）和任务模式（路由判断）
3. 维护模式：扫描 skills 目录 → 读取 SKILL.md → 生成中文索引
4. 任务模式：理解任务 → **先读取 agent list** → 推荐实际存在的 agent → 推荐 skill
5. 简单任务 1 个 skill、中等 2 个、复杂最多 3 个
6. 最强模型使用规则：只用于高价值任务
7. 安全规则：不高权限自动执行、不循环派单
8. 中文触发示例（至少 10 个）

## 关键规则

- **必须先读取 agent list，再推荐实际存在的 agent**
- 不凭空编造 agent 名称
- 不假设所有用户都有相同的 agent 结构
- 作者案例只作为 Example，标记"仅供参考"

## 参考

可以基于 `skills/cn-skill-router/SKILL.md` 创建，但需要：
- 去个人化
- 将所有 agent 引用改为"先读取再推荐"
- 将作者的 7-agent 案例只放在 Example Setup 章节
