---
name: cn-skill-router
description: 中文任务路由与技能导航。Use when the user speaks Chinese, is unsure which installed skill to use, wants to scan/update the current skills directory, or wants the agent to choose suitable skills before continuing a real task.
---

# CN Skill Router — 中文任务路由与技能导航

## 概述

cn-skill-router 是**元技能（meta-skill）**。它在用户和已安装的 skills 之间充当导航层。

两种工作模式：
1. **维护模式**：扫描当前环境中已安装的 skills，生成/更新中文索引和推荐组合表。
2. **任务模式**：根据用户的真实任务目标，从已安装的 skills 中选择最合适的 1-3 个，然后继续执行真实任务。

**本 skill 不直接执行 Multica CLI。** 实际 CLI 操作使用 `multica-cli-operator`。

---

## 模式一：维护模式

### 触发条件
用户说："更新技能推荐表"、"扫描已安装 skill"、"刷新技能索引"等。

### 执行步骤
1. 扫描 `~/.claude/skills/`、当前项目 `.claude/skills/` 和系统已注册 skill 列表
2. 读取每个 SKILL.md 的 frontmatter（name、description）
3. 为每个 skill 生成中文说明
4. 标记确定性：✅ 已确认 / ⚠️ 需人工确认 / ❌ 未找到
5. 输出中文索引表和推荐组合表

---

## 模式二：任务模式

### 触发条件
用户说："使用 cn-skill-router 判断"、"帮我选择 skill"、"这个任务用什么 skill"等。

### 执行步骤
1. 用一句话理解用户任务目标
2. 判断任务类型（编程/写作/审查/调研等）
3. **先读取当前 agent list**：`multica agent list --output json`
4. 从**实际存在的 agent** 中选择最合适的
5. 选择 1-3 个 skill（简单 1 个、中等 2 个、复杂最多 3 个）
6. 说明原因（为什么选这些，为什么不选其他）
7. 给出执行顺序
8. **继续执行任务，不要只停留在推荐**

---

## 重要规则

1. cn-skill-router 是导航层，不是替代品。判断 → 推荐 → 继续执行。
2. 不要每次都推荐所有 skill。优先最少但足够。
3. **先读 agent list，再推荐实际存在的 agent。不凭空编造 agent。**
4. 对高权限 skill 标注风险。
5. 不要凭空编造不存在的 skill。
6. 只允许读取 skill 文件和创建/更新 cn-skill-router 相关文件。

---

## 最强模型使用规则

只有在以下情况才推荐最强模型（GPT/Opus 等）：
- 架构级别的审查或重构
- 安全关键问题
- Pro 两次尝试仍无法解决的复杂问题
- 最终代码审查或论文终审

绝对不推荐最强模型的场景：普通润色、小 JSON、单文件小 bug、简单 README。

---

## 输出格式（任务模式）

```
## 任务理解
[一句话]

## 推荐使用的 Skill
| Skill | 用途 | 为什么选它 |

## 不使用的 Skill
| Skill | 为什么不选 |

## 执行顺序
1. 2. 3.

## 继续执行
[直接开始完成用户的真实任务]
```

---

## Example Setup（仅供参考）

以下是一个可能的 agent 结构示例，**不是标准答案**：

| Agent | 定位 | 适合任务 |
|-------|------|---------|
| Manager | 总调度 | 任务路由、配置维护 |
| Coding-Light | 编程轻任务 | 简单报错、JSON、配置 |
| Coding-Main | 编程主力 | 常规 bug、功能开发 |
| Coding-Review | 编程审查 | 架构、安全、最终 review |
| Writing-Light | 写作轻任务 | 提纲、笔记、短文 |
| Writing-Main | 写作主力 | 论文、报告、文档 |
| Writing-Review | 写作终审 | 论文终审、重要报告审查 |

**你可以完全不同：** 2 个 agent 也行、3 个也行、不区分写作编程也行。
**你的 agent 结构应该由你的模型、skill、预算和任务类型决定。**
