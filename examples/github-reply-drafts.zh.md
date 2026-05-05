# GitHub 回复草稿

> 以下草稿用于在不同 GitHub Discussions 分类中分享。
> 使用前请替换 `<占位符>` 并完成脱敏检查。

---

## Show and tell（最推荐）

**标题：** Multica Agent Workflow Template — 一个帮助你设计多 agent + skill 工作流的方法论模板

**内容草稿：**

> 大家好！我做了一个开源模板项目，帮助 Multica 用户系统化设计自己的多 agent + skill 工作流。
>
> 这不是一份固定的配置清单。它提供：
> - 5 种 agent 结构方案（从 2-agent 极简到双线三层）
> - Skill 映射方法和分配原则
> - Manager agent 模式和完整指令模板
> - CLI 派单流程和 issue 模板
> - 测试流程（从健康检查到完整链路）
> - 脱敏检查清单
>
> 项目的核心观点是：**你的 agent 结构应该由你的模型、skill、预算和任务类型决定。** 别人的配置只是参考，不是标准答案。
>
> 仓库：<你的 GitHub 链接>

---

## Ideas（回复 agent-skill pack 相关讨论）

**内容草稿：**

> 关于 agent-skill pack 和 CLI orchestration，我最近做了一个实践项目，核心经验是：
>
> 1. Skill 需要分类分配给 agent，不是每个 agent 都开全部 skill
> 2. 路由类 skill（如 cn-skill-router）和 CLI 类 skill（如 multica-cli-operator）只给 manager agent
> 3. 高权限 skill 需要谨慎分配
> 4. 测试流程很重要：先测轻任务 → 再测分诊 → 再测完整链路
>
> 详细方案和模板在这里：<你的 GitHub 链接>

---

## General（分享中文用户体验）

**内容草稿：**

> 作为中文用户，在 Multica 上搭建了一套多 agent 工作流后，发现几个关键点：
>
> 1. 中文任务路由 skill（cn-skill-router）很有用：自动判断任务类型、推荐 agent 和 skill
> 2. Agent 之间通过 issue 协调比想象中更稳定
> 3. 不是所有任务都需要最强模型——按能力分层能省不少额度
>
> 我把方法论和模板整理成了开源项目：<你的 GitHub 链接>
> 欢迎大家参考和提建议！

---

## Q&A（只回答问题，不推广）

**场景：** 有人问"如何让多个 agent 协作"

**回复草稿：**

> Agent 之间的协作方式是通过 issue 系统，不是同步函数调用。基本流程是：
> 1. Manager 创建 issue
> 2. 分配 assignee
> 3. Worker 在 issue comment 中回复结果
> 4. Manager 通过 runs 和 run-messages 读取结果
>
> 你用 `multica issue create` 和 `multica issue assign` 就可以实现。如果 agent 较多，建议配一个 skill router 来自动判断该分配给谁。
>
> 具体命令和模板可以参考 `multica issue create --help`。

---

## Issues（仅 bug 或 feature request）

**不要在 Issues 中做推广。** 只在遇到实际的 bug 或有明确的 feature request 时使用。
