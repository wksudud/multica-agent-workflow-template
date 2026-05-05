# Skill 映射示例

> 以下示例展示如何将不同类型的 skill 分配给不同的 agent。
> 你的实际 skill 列表可能不同，请根据你的情况调整。

---

## 示例 1：2-agent 结构下的 skill 分配

### Manager
- cn-skill-router
- multica-cli-operator

### Worker（承担所有执行任务）
- karpathy-guidelines
- Coding Lead
- doc-coauthoring
- Codebase Documenter
- OpenClaw JSON Toolkit

注意：Worker 只有 5 个 skill。不用全部 skill 都开。

---

## 示例 2：3-agent 分层下的 skill 分配

### Light Agent
- karpathy-guidelines
- OpenClaw JSON Toolkit
- Obsidian Markdown
- Bookmark Keeper
- 深度话题调研工作流

### Main Agent
- Coding Lead
- karpathy-guidelines
- doc-coauthoring
- Humanizer
- Codebase Documenter
- Agent Git Oracle
- SpecVibe
- Aj Github

### Review Agent
- Agent Git Oracle
- Pre Publish Security
- security-review
- karpathy-guidelines

---

## 示例 3：写作+编程双线下的 skill 分配

### 写作线

| Agent | Skills |
|-------|--------|
| Writing-Light | doc-coauthoring, Obsidian Markdown, Bookmark Keeper, 深度话题调研工作流 |
| Writing-Main | doc-coauthoring, Humanizer, Codebase Documenter, 深度话题调研工作流 |
| Writing-Review | doc-coauthoring, Humanizer, Pre Publish Security |

### 编程线

| Agent | Skills |
|-------|--------|
| Coding-Light | karpathy-guidelines, OpenClaw JSON Toolkit, skill-guard |
| Coding-Main | Coding Lead, karpathy-guidelines, Agent Git Oracle, Aj Github, SpecVibe, OpenClaw JSON Toolkit |
| Coding-Review | Agent Git Oracle, karpathy-guidelines, Pre Publish Security, security-review |

### Manager（如有）
- cn-skill-router
- multica-cli-operator

---

## 分配原则总结

1. 路由类 skill → Manager（且只给 Manager）
2. CLI 类 skill → Manager（且只给 Manager）
3. 编程类 skill → Coding agent(s)
4. 写作类 skill → Writing agent(s)
5. 审查类 skill → Review agent(s)
6. 高权限 skill → 仅给明确需要的 agent，标注风险
7. 一个 skill 可以分配给多个 agent（如 karpathy-guidelines）
8. 每个 agent 的 skill 数量建议 3-10 个
