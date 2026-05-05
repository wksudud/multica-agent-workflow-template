# 07 — 作者个人案例（Example Case Study）

> **这不是标准答案。** 这是作者根据自己的情况设计的工作流。
> 你的模型、skill、预算和任务类型可能与作者完全不同。
> 请将这个案例视为参考，而不是模板。

---

## 作者的情况

作者的需求：

- 同时做编程和写作任务
- 有多个模型（Flash / Pro / GPT），能力和成本不同
- 安装了较多 skill（15+）
- 希望中文输入时能自动判断 agent 和 skill
- 需要一个总调度 agent 统一管理

---

## 最终采用的 Agent 结构

作者最终采用了 **1 个 Manager + 3 个 Coding + 3 个 Writing = 7 个 agent** 的结构。

### Manager（1 个）

| Agent | 模型 | 职责 |
|-------|------|------|
| 总调度官 | Pro | 任务路由、agent 调度、配置维护、结果汇总 |

### 编程线（3 个）

| Agent | 模型 | 定位 | 适合 |
|-------|------|------|------|
| 轻修分诊员 | Flash | 低成本分诊和轻量修复 | 简单报错、JSON、配置、任务复杂度判断 |
| 开发主力 | Pro | 主力编程执行 | 常规 bug、功能实现、多文件修改、测试 |
| 架构审查官 | GPT | 最高能力专家 | 复杂 bug、架构审查、安全审查、最终 review |

### 写作线（3 个）

| Agent | 模型 | 定位 | 适合 |
|-------|------|------|------|
| 资料整理员 | Flash | 低成本写作和整理 | 提纲、初稿、笔记、短文润色 |
| 论文主笔 | Pro | 主力写作执行 | 论文正文、实验报告、项目文档 |
| 终审编辑 | GPT | 最高质量审查 | 论文终审、重要报告终审、逻辑检查 |

---

## 作者的关键 Skill

| Skill | 分配给 |
|-------|-------|
| cn-skill-router | Manager |
| multica-cli-operator | Manager |
| Coding Lead | 所有 Coding agent |
| karpathy-guidelines | 所有 Coding agent |
| doc-coauthoring | 所有 Writing agent |
| Humanizer | Writing-Main、Writing-Review |

---

## 为什么选择这个结构？

1. **任务类型双线分明**：编程和写作是两类完全不同的任务，分开 agent 让指令更聚焦
2. **三层成本控制**：Flash 处理简单任务（省钱），Pro 处理主力任务，GPT 只用于高价值任务（省额度）
3. **Manager 解耦**：Manager 不执行具体任务，只做路由和汇总
4. **有兜底机制**：如果 Pro 处理不了，可以升级到 GPT

---

## 你也可以不这么做

作者的 7-agent 结构有很多前提条件（模型多、skill 多、任务类型多样）。

**如果你只有 1-2 个模型，完全不需要 7 个 agent。**

- 你完全可以用 2 个 agent（方案 A）
- 你可以不区分写作和编程
- 你可以不设 manager
- 你可以不用 Flash，只用 Pro 和 GPT 两层

**请参考 `docs/02-how-to-design-agents.zh.md` 选择适合你的方案。**
