# 项目启动包示例

适合中大型任务：新建一个网站、agent 项目、桌面工具、研究报告或需要多 agent 协作的工程任务。

目标不是制造很多文档，而是让后续 agent 不需要读完整聊天记录，也能知道怎么继续。

---

## 推荐文件

```text
<project>/
  requirements.md        # 用户目标、场景、范围、非目标、验收摘要
  technical-design.md    # 技术方向、数据/状态、接口、依赖、风险、验证计划
  AGENTS.md              # 给 agent 的导航入口，只放薄规则和文件索引
  prompt.md              # 给下一轮执行 agent 的紧凑任务提示
  ops/
    acceptance.md        # 详细验收标准和边界情况
    agent-brief.md       # 可直接派单的简短 brief
```

---

## requirements.md 最小结构

```markdown
# Requirements

## Goal
<这个项目要为谁解决什么问题>

## User Scenario
<用户如何使用它>

## In Scope
- <第一版必须包含什么>

## Out of Scope
- <第一版不做什么>

## Acceptance Summary
- <用户可见的完成标准>

## Open Questions
- <还需要确认的问题>
```

---

## technical-design.md 最小结构

```markdown
# Technical Design

## Surfaces
<CLI / Web / Streamlit / API / Desktop 等>

## Data and State
<输入、输出、缓存、状态保存方式>

## Components
- <组件 1>: <职责>
- <组件 2>: <职责>

## Dependencies
- <依赖和原因>

## Risks
- <技术风险、隐私风险、发布风险>

## Verification Plan
- <测试命令、浏览器检查、人工审查点>
```

---

## AGENTS.md 最小结构

```markdown
# Agent Navigation

## Read First
- requirements.md
- technical-design.md
- prompt.md
- ops/acceptance.md

## Boundaries
- Do not publish, push, delete, or change accounts without confirmation.
- Do not expose secrets, private data, runtime logs, or local machine paths.

## Finish Rule
- Return changed_paths, verification, assumptions, blockers, and next_action.
```

---

## prompt.md 最小结构

```markdown
# Execution Prompt

Goal:
<本轮要完成什么>

Allowed files:
- <允许修改的文件>

Acceptance:
- <验收标准>

Verification:
- <必须运行的检查>

Stop if:
- <遇到什么必须停止并询问>
```

---

## 什么时候不要用启动包？

- 只改一行错别字
- 只问一个概念
- 只需要临时总结
- 任务没有后续维护价值

对小任务，直接写清目标、限制和验收即可。
