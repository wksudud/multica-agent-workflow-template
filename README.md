# Multica Agent Workflow Template

> **这不是一份固定配置清单。** 这是一个帮助你设计**你自己的** Multica 多 agent + skill 工作流的方法论模板。

---

## 项目简介

Multica Agent Workflow Template 帮助你：

- 根据自己拥有的模型、预算、skill 和任务类型，**设计适合自己的 agent 结构**
- 建立从"用户说一句话"到"正确的 agent 执行正确任务"的完整链路
- 理解 agent 之间如何通过 issue 系统稳定协作（而非假装能同步调用）

项目面向中文用户，也适合任何想系统化设计 Multica 工作流的人。

---

## 适用人群

| 你可能是…… | 这个项目能帮你…… |
|-----------|---------------|
| 安装了多个 skill，但不知道什么时候用哪个 | 建立 skill → agent → 任务类型的映射 |
| 有多个模型（便宜的 + 主力的 + 最强的），不知道怎么分任务 | 设计按能力/成本分层的 agent 结构 |
| 想用一个 manager agent 管理多个 worker agent | 提供 manager pattern 的完整模板和指令 |
| 看到别人的复杂配置想复制，但不确定是否适合自己 | **告诉你不要照抄，而是理解原理后自己设计** |
| 想把自己的 Multica 工作流整理成可复用的开源模板 | 提供项目结构、文档模板和分享策略 |

---

## 这不是固定配置

**请反复记住以下几点：**

- 你可以只创建 **2 个 agent**。
- 你可以创建 **3 个 agent**。
- 你可以创建 **6 个或 7 个 agent**。
- 你可以**完全不区分写作和编程**。
- 你可以**只用一个强模型**，也可以用"便宜模型 + 主力模型 + 审查模型"三层。
- 你可以**不设 manager agent**，手动分配任务。
- **示例中的 7-agent 配置只是一个案例，不是标准答案。**

---

## 工作流总览

```
用户说一段话
      ↓
Manager Agent（可选）
  读取 agent 配置，理解任务
      ↓
Task Router Skill
  判断任务类型、复杂度、推荐 agent 和 skill
      ↓
CLI Operator Skill
  创建 issue → 分配 agent → 查看 runs
      ↓
Worker Agent
  接收 issue → 执行任务 → 回复结果
      ↓
Manager Agent
  汇总结果 → 反馈给用户
```

---

## 快速开始

### 第一步：列出你的模型

| 模型名称 | 能力评级 | 价格 | 额度限制 |
|---------|---------|------|---------|
| （你的模型1） | （强/中/经济） | | |
| （你的模型2） | | | |

### 第二步：列出你的 skill

运行 `multica agent list --output json` 查看已安装的 skills。

### 第三步：决定 agent 数量

从 2 个开始通常是好的选择。见 `docs/02-how-to-design-agents.zh.md`。

### 第四步：决定是否需要 manager agent

如果你只有 1-2 个 agent，可以直接手动分配。agent 多了再考虑 manager。

### 第五步：加载初始化向导（推荐）

加载 `multica-workflow-bootstrapper` skill，它会引导你完成需求访谈、配置推荐和 agent 指令生成。

如果该 skill 尚未安装，使用 `prompts/08-create-workflow-bootstrapper.zh.md` 创建。

### 第六步：创建 skill router 和 CLI operator

根据 bootstrapper 的指引，使用以下提示词创建辅助 skill：
- `prompts/05-create-cn-skill-router.zh.md`
- `prompts/04-create-multica-cli-operator.zh.md`

### 第七步：低风险测试

见 `docs/06-testing-workflow.zh.md`，先测试分诊和短文写作，再测试代码修复。

### 第八步：确认无问题后，开始真实任务

---

## 作者案例

作者的个人配置是一个 7-agent + 双线（写作 × 编程）× 三层（Flash × Pro × GPT）的结构，包含一个 manager agent 负责总调度。

详见 `docs/07-case-study-my-setup.zh.md`。

**再次强调：这只是一个案例，不是标准答案。** 你完全可以根据自己的情况采用更简单或更复杂的结构。

---

## 安全提醒

- 不要上传 token、API key、cookie、私钥
- 不要上传本地绝对路径
- 不要上传 workspace 名称或 slug
- 不要上传邮箱或真实账号名
- 不要让 agent 自动 push 或发布
- 高权限 skill（鼠标控制、浏览器控制、系统操作）需谨慎分配
- 发 GitHub 前逐项检查个人信息、密钥、路径和 workspace 信息是否已替换为占位符

---

## 项目结构

```
multica-agent-workflow-template/
  README.md
  docs/
    01-overview.zh.md               # 问题总览
    02-how-to-design-agents.zh.md   # Agent 设计方案
    03-how-to-map-skills.zh.md      # Skill 映射方法
    04-manager-agent-pattern.zh.md  # Manager 模式
    05-multica-cli-dispatch.zh.md   # CLI 派单说明
    06-testing-workflow.zh.md       # 测试流程
    07-case-study-my-setup.zh.md    # 作者案例（仅供参考）
    08-troubleshooting.zh.md        # 常见排错
  skills/
    multica-workflow-bootstrapper/  # 工作流初始化向导（入口 skill）
      SKILL.md
    _implementation-details/        # 实现细节（已被 skill 取代）
      README.md                     # 说明本目录的用途
      cn-skill-router/SKILL.md      # 中文路由 skill 参考实现
      multica-cli-operator/SKILL.md # CLI 操作 skill 参考实现
  prompts/
    01-discuss-your-needs-with-ai.zh.md
    02-design-agent-structure.zh.md
    03-create-manager-agent.zh.md
    04-create-multica-cli-operator.zh.md
    05-create-cn-skill-router.zh.md
    06-test-multica-dispatch.zh.md
    07-prepare-github-sharing.zh.md
    08-create-workflow-bootstrapper.zh.md
  examples/
    agent-structures.zh.md          # Agent 结构示例
    skill-mapping-examples.zh.md    # Skill 映射示例
    issue-description-templates.zh.md # Issue 模板
    github-reply-drafts.zh.md       # GitHub 回复草稿
  SECURITY.md
  LICENSE
  .gitignore
```

> **关于 `skills/_implementation-details/`**：本目录包含 `cn-skill-router` 和 `multica-cli-operator` 的 SKILL.md 参考实现。
> 这些是**已被 skill 取代的具体实施细节**。新用户不需要手动复制这些文件——使用 `multica-workflow-bootstrapper`（入口 skill）即可自动引导完成初始化。
> 该目录仅供想了解 skill 内部设计逻辑或手动定制的用户参考。

---

## GitHub 分享策略

如果你想在 GitHub 上分享你的工作流经验：

1. **Show and tell**（最推荐）：展示完整工作流和设计思路
2. **Ideas**：回复与 agent-skill pack / CLI orchestration 相关的讨论
3. **General**：简单分享中文用户体验
4. **Q&A**：只回答问题，不推广
5. **Issues**：只在遇到 bug 或 feature request 时使用

参考地址：
- 仓库主页：[https://github.com/multica-ai/multica](https://github.com/multica-ai/multica)
- Discussions：[https://github.com/multica-ai/multica/discussions](https://github.com/multica-ai/multica/discussions)

---

## License

MIT
