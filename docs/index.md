# Multica Agent Workflow Template

> 基于 [Multica](https://github.com/multica-ai/multica) 官方软件构建的多 agent + skill 工作流设计方法论模板。
>
> **这不是一份固定配置清单。** 这是一个帮助你设计**你自己的** Multica 多 agent + skill 工作流的方法论模板。

---

## 项目简介

Multica Agent Workflow Template 帮助你：

- 根据自己拥有的模型、预算、skill 和任务类型，**设计适合自己的 agent 结构**
- 建立从"用户说一句话"到"正确的 agent 执行正确任务"的完整链路
- 理解 agent 之间如何通过 issue 系统稳定协作（而非假装能同步调用）

项目面向中文用户，也适合任何想系统化设计 Multica 工作流的人。

---

## 快速开始

1. 从 [Multica 官方仓库](https://github.com/multica-ai/multica) 安装并运行 `multica setup`
2. 列出你的模型和 skill
3. 阅读 [问题总览](01-overview.zh.md) 了解核心概念
4. 按 [Agent 设计方案](02-how-to-design-agents.zh.md) 设计自己的 agent 结构
5. 加载 `multica-workflow-bootstrapper` 初始化向导

---

## 文档导航

| 文档 | 内容 |
|------|------|
| [问题总览](01-overview.zh.md) | 项目解决的核心问题 |
| [Agent 设计方案](02-how-to-design-agents.zh.md) | 如何设计 agent 数量和结构 |
| [Skill 映射方法](03-how-to-map-skills.zh.md) | skill → agent → 任务类型映射 |
| [Manager 模式](04-manager-agent-pattern.zh.md) | Manager agent 完整模板 |
| [CLI 派单说明](05-multica-cli-dispatch.zh.md) | CLI 操作和 issue 派单 |
| [测试流程](06-testing-workflow.zh.md) | 低风险测试方法 |
| [作者案例](07-case-study-my-setup.zh.md) | 7-agent 配置案例（仅供参考） |
| [常见排错](08-troubleshooting.zh.md) | 常见问题和解决方案 |

## 免责声明

> **本项目是方法论文档集，不是即装即用的配置。** 所有 agent 配置案例仅供参考，请根据自身需求设计。引用的第三方 skill 仅提供链接，不包含在本仓库中。详见 [README 免责声明](https://github.com/wksudud/multica-agent-workflow-template#免责声明)。
> **本文档大部分由 AI agent 辅助生成，可能存在错误。** 请在参考和使用前自行验证关键内容。

## 安全提醒

- 不要上传 token、API key、cookie、私钥
- 不要让 agent 自动 push 或发布
- 高权限 skill 需谨慎分配
- 发 GitHub 前逐项检查敏感信息
