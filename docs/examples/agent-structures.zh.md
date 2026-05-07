# Agent 结构示例

> 以下所有结构都是**示例**，不是必须照抄的标准配置。
> 选择最适合你的，或者混合搭配。

---

## 示例 1：2-agent 极简

| Agent | 模型 | 职责 |
|-------|------|------|
| Manager | Pro | 理解任务，路由判断，创建 issue，汇总结果 |
| Worker | Pro | 执行所有类型的任务（写作+编程） |

适合：刚开始、skill 少、任务类型单一。

---

## 示例 2：3-agent 分层

| Agent | 模型 | 职责 |
|-------|------|------|
| Light | Flash | 轻任务、分诊、初稿、笔记 |
| Main | Pro | 主力执行（写作+编程） |
| Review | GPT/Opus | 安全审查、最终审查 |

适合：有多个模型、想按能力分层。

---

## 示例 3：4-agent 按领域拆分

| Agent | 模型 | 职责 |
|-------|------|------|
| Writer | Pro | 写作、润色、翻译 |
| Coder | Pro | 编程、调试、文档 |
| Researcher | Flash | 调研、分析、综述 |
| Reviewer | GPT/Opus | 质量审查 |

适合：任务类型多样，但不想分太细。

---

## 示例 4：6-agent 双线三层

### 写作线
| Agent | 模型 | 职责 |
|-------|------|------|
| Writing-Light | Flash | 提纲、笔记、短文、整理 |
| Writing-Main | Pro | 论文、报告、文档 |
| Writing-Review | GPT/Opus | 终审、逻辑检查 |

### 编程线
| Agent | 模型 | 职责 |
|-------|------|------|
| Coding-Light | Flash | 简单报错、JSON、配置 |
| Coding-Main | Pro | 常规 bug、功能开发、测试 |
| Coding-Review | GPT/Opus | 架构、安全、最终 review |

适合：精细分工、追求成本最优、有较多模型。

---

## 示例 5：7-agent + Manager（作者个人案例）

1 个 Manager + 6 个 Worker（双线 × 三层）。

详见 `docs/07-case-study-my-setup.zh.md`。

**这不是标准答案，只是作者的个人配置。**

---

## 选择指南

| 如果你…… | 建议 |
|----------|------|
| 刚开始 | 示例 1 或 2 |
| 写作和编程都做 | 示例 3 或 4 |
| 想精细控制成本 | 示例 4 |
| 不确定 | 从示例 2 开始，用一段时间再调整 |
