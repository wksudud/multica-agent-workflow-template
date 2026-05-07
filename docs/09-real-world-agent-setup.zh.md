# 09 — 真实案例：作者的 11-Agent 配置

> **这不是一份推荐配置清单，更不是标准答案。** 这是作者在当前时间点的个人真实配置，仅供理解"如何根据自己的需求设计 agent 结构"时参考。
>
> 你的模型、预算、skill 和任务类型可能完全不同——**请理解原理后自行设计，不要照抄。**

---

## 架构总览

作者采用 **双线（写作 × 编程）+ 媒体 + 管理层** 的 11-agent 结构，配合 **三层模型（Flash × Pro × GPT）** 按能力与成本分层。

```
                     ┌──────────────────┐
                     │   管理层 (3)       │
                     │  M-Pro / R-GPT /  │
                     │  P-GPT            │
                     └──────┬───────────┘
                            │ 调度、审查、计划
          ┌─────────────────┼─────────────────┐
          │                 │                 │
    ┌─────┴─────┐   ┌──────┴──────┐   ┌──────┴──────┐
    │ 写作线 (3) │   │ 编程线 (3)   │   │ 媒体线 (2)   │
    │ W-Flash   │   │ C-Flash     │   │ I-Pro       │
    │ W-Pro     │   │ C-Pro       │   │ Web-Pro     │
    │ W-GPT     │   │ C-GPT       │   │             │
    └───────────┘   └─────────────┘   └─────────────┘
```

### 三层模型策略

| 层级 | 模型 | 适用 agent | 用途 |
|------|------|-----------|------|
| Flash（经济） | deepseek-v4-flash | W-Flash, C-Flash | 低成本分诊、轻任务、初筛 |
| Pro（主力） | deepseek-v4-pro | W-Pro, C-Pro, M-Pro, Web-Pro | 常规开发、写作、调度 |
| GPT（最强） | gpt-5.4 / gpt-5.5 | W-GPT, C-GPT, R-GPT, P-GPT, I-Pro | 终审、架构、安全、图像 |

---

## 写作线

写作线负责文档、论文、报告等中文写作任务，按复杂度逐级升级。

**升级路径：W-Flash → W-Pro → W-GPT**

### W-Flash-资料整理员
- **模型**：deepseek-v4-flash（低成本）
- **角色**：资料整理、轻写作、笔记和链接管理
- **适合**：提纲、摘要、短文润色、Obsidian 笔记、网页/资料分类
- **不适合**：深度调研、长文档、毕业论文终稿、安全审查
- **Skills**：Bookmark Keeper, Humanizer, Obsidian Markdown

### W-Pro-论文主笔
- **模型**：deepseek-v4-pro（主力）
- **角色**：论文、报告和长文档主笔
- **适合**：章节扩写、报告正文、技术综述、文档结构化写作
- **不适合**：最终高风险提交审查、代码实现、图片生成
- **Skills**：Bookmark Keeper, Codebase Documenter, doc-coauthoring, Humanizer, Obsidian Markdown, 深度话题调研工作流

### W-GPT-终审编辑
- **模型**：gpt-5.4（最强）
- **角色**：重要写作终审和逻辑编辑
- **适合**：毕业论文/重要报告终审、逻辑漏洞检查、AI 味过重修复
- **不适合**：普通短文、小润色、代码实现、发布执行
- **Skills**：doc-coauthoring, Humanizer
- **⚠️ 注意**：GPT 额度高，仅用于高价值终审任务

---

## 编程线

编程线负责代码开发、审查和安全，按复杂度逐级升级。

**升级路径：C-Flash → C-Pro → C-GPT**

### C-Flash-轻修分诊员
- **模型**：deepseek-v4-flash（低成本）
- **角色**：轻量代码分诊、配置和小修
- **适合**：简单报错、JSON/配置问题、小脚本、skill 安全初筛
- **不适合**：多文件重构、复杂架构、安全最终审查、系统高风险操作
- **Skills**：Coding Lead, karpathy-guidelines, OpenClaw JSON Toolkit, skill-guard
- **⚠️ skill-guard**：仅用于安装新 skill 前的安全初筛，不做最终审查

### C-Pro-开发主力
- **模型**：deepseek-v4-pro（主力）
- **角色**：常规代码开发、功能实现和项目文档
- **适合**：功能实现、多文件修改、测试修复、README/API 文档、GitHub/CI 处理
- **不适合**：最终架构审查、高风险安全审查、发布决策
- **Skills**：Agent Git Oracle, Aj Github, Codebase Documenter, Coding Lead, karpathy-guidelines, OpenClaw JSON Toolkit, SpecVibe

### C-GPT-架构审查官
- **模型**：gpt-5.4（最强）
- **角色**：架构、复杂代码和安全审查
- **适合**：架构设计/审查、复杂跨模块问题、安全风险、发布前代码审查
- **不适合**：普通小修、简单文档生成
- **Skills**：Agent Git Oracle, Aj Github, Coding Lead, karpathy-guidelines, Pre Publish Security, SpecVibe
- **⚠️ Pre Publish Security**：发布前安全检查，**执行前需人工确认**

---

## 媒体线

### I-Pro-图像生成师
- **模型**：gpt-5.5（最强）
- **角色**：图像生成、图片编辑与视觉提示词
- **适合**：海报、封面、头像、插画、图标概念、参考图改造
- **不适合**：网页/代码实现、文字排版要求高的海报
- **Skills**：image-generation-operator, Nano Banana Pro, Openai Image Gen
- **⚠️ 注意**：生成前检查安全/版权/肖像/商标风险；真实调用前需确认数量和额度

### Web-Pro-网页生成师
- **模型**：deepseek-v4-pro（主力）
- **角色**：网页生成、前端页面、交互原型
- **适合**：网页实现、前端页面、小型 Web App、交互原型
- **不适合**：后端大系统、支付、真实账号权限、发布上线
- **Skills**：Anthropic Frontend Design, Coding Lead, Frontend Design Extractor, karpathy-guidelines, SpecVibe

---

## 管理层

### M-Pro-调度官
- **模型**：deepseek-v4-pro（主力）
- **角色**：中文团队调度与任务路由
- **职责**：理解目标 → 分级 → 计划 → 创建 issue → 派单 → 汇总
- **边界**：不写代码、不写论文、不生成图片、不做网页实现
- **Skills**：cn-skill-router, multica-cli-operator

### R-GPT-审查验收官
- **模型**：gpt-5.4（最强）
- **角色**：计划审查、结果验收、风险把关
- **职责**：审查计划、验收结果、发布前安全检查
- **边界**：只审查和验收，不抢计划、不抢执行
- **Skills**：cn-skill-router, multica-cli-operator, Pre Publish Security, review-acceptance-operator
- **⚠️ Pre Publish Security**：发布前最终安全审查，**执行前需人工确认**

### P-GPT-详细计划生成师
- **模型**：gpt-5.4（最强）
- **角色**：复杂任务详细计划生成
- **职责**：在复杂/高风险任务执行前生成完整 Phase 计划
- **边界**：不执行任务、不创建 issue、不修改 agent/skill
- **Skills**：cn-skill-router, karpathy-guidelines, multica-cli-operator, Pre Publish Security, review-acceptance-operator, SpecVibe

---

## 设计理念

### 为什么双线分离？

写作和编程是两种完全不同的能力要求，分开后：
- 每条线的 agent 指令更聚焦，不会因为"既能写又能码"导致判断混乱
- 模型选择更精准（写作线不需要 Coding Lead，编程线不需要 Humanizer）
- 升级路径更清晰

### 为什么三层模型？

| 层级 | 原则 |
|------|------|
| Flash | 先用便宜模型分诊，判断任务复杂度 |
| Pro | 常规任务用主力模型，性价比最高 |
| GPT | 仅用于高价值终审/架构/安全，避免浪费额度 |

### 为什么需要 manager agent？

只有 1-2 个 agent 时可以手动分配。agent 超过 5 个后，manager agent 的价值开始体现：
- 自动判断任务属于写作线还是编程线
- 自动判断复杂度，选择合适的 agent 层级
- 创建 issue、派单、追踪进度

**不是必须的。** 如果你只有 2-3 个 agent，手动分配可能更高效。

### 为什么设审查官（R-GPT）？

任何系统都可能出错。R-GPT 作为独立审查节点：
- 计划审查：执行前找出遗漏和风险
- 结果验收：执行后确认交付质量
- 安全把关：发布前最终安全审查

---

## 安全提醒

以下 skill 涉及高风险操作，分配和使用时需特别注意：

| Skill | 风险 | 防护 |
|-------|------|------|
| Pre Publish Security | 发布前安全审查 | 仅分配给 GPT 级 agent，执行前需人工确认 |
| skill-guard | skill 安全初筛 | 仅用于安装前检查，不做最终审查 |
| Aj Github | GitHub 操作（issue/PR/CI） | 不分配 push/delete 权限给 agent |
| multica-cli-operator | Multica CLI 操作 | 限制为只读操作，写操作需人工确认 |

**通用安全规则**（适用于所有 agent 的指令中均包含）：
- 不上传 API key、token、cookie、私钥
- 不自动 push 或发布
- 不在未确认时做删除、系统权限修改、外部发帖
- 高风险操作必须等待用户明确确认

---

## 与你的配置有何不同？

再次强调，这只是一个案例。你的配置可能：

- 更简单（2-3 个 agent，不区分写作/编程线）
- 只有一层模型（全部用同一个模型）
- 没有 manager agent（手动分配）
- 没有图像/网页 agent（不需要媒体产出）
- 使用完全不同的模型（Claude / Gemini / Qwen 等）

**这些变化都是合理且推荐的。** 核心方法论不是"照抄 11 agent"，而是理解以下原则后自行设计：
1. 按任务类型分 agent（写作 / 编程 / 媒体）
2. 按能力和成本分层（便宜的做分诊，主力的做常规，最强的做审查）
3. 每个 agent 职责边界清晰（知道什么该做什么不该做）
4. 有独立的审查节点（人或 agent）
5. 高风险操作有明确的人工确认点

---


---

## 附录：Skill 来源一览

以下标注每个 skill 的来源。ClawHub 公开 skill 仅提供链接，**不上传 skill 文件**；标注"自建"的为作者自定义。

### 写作相关

| Skill | 来源 | 获取方式 |
|-------|------|----------|
| Humanizer | ClawHub | `clawhub install humanizer` |
| Obsidian Markdown | ClawHub | `clawhub install obsidian` |
| Bookmark Keeper | 待确认 | ClawHub 有类似 `karakeep` / `gog` |
| doc-coauthoring | Anthropic 官方 | Claude Code 内置 |
| Codebase Documenter | ClawHub | `clawhub install veeramanikandanr48/codebase-documenter` |
| 深度话题调研工作流 | ClawHub | `clawhub install realpda/deep-topic-research` |

### 编程相关

| Skill | 来源 | 获取方式 |
|-------|------|----------|
| karpathy-guidelines | ClawHub | `clawhub install karpathy-guidelines` |
| Aj Github | ClawHub | `clawhub install aj-github` |
| Coding Lead | ClawHub | `clawhub install beyound87/coding-lead` |
| SpecVibe | ClawHub | `clawhub install badideal-2046/specvibe` |
| Agent Git Oracle | ClawHub | `clawhub install tmstudio667-commits/agent-git-oracle` |
| OpenClaw JSON Toolkit | ClawHub | `clawhub install yedanyagamiai-cmd/openclaw-json-toolkit` |
| skill-guard | 自建 | 无公开下载 |

### 媒体/设计相关

| Skill | 来源 | 获取方式 |
|-------|------|----------|
| Nano Banana Pro | ClawHub | `clawhub install nano-banana-pro` |
| Openai Image Gen | 待确认 | ClawHub 有多个 `openai-image` 变体 |
| Anthropic Frontend Design | Anthropic 官方 | `px skills add anthropic/frontend-design` |
| Frontend Design Extractor | ClawHub | `clawhub install xsir0/frontend-design-extractor` |
| image-generation-operator | 自建 | 无公开下载（封装 Nano Banana + OpenAI 调度） |

### 管理与安全

| Skill | 来源 | 获取方式 |
|-------|------|----------|
| Pre Publish Security | ClawHub | `clawhub install pre-publish-security` |
| cn-skill-router | 自建 | 见本仓库 `skills/_implementation-details/cn-skill-router/` |
| multica-cli-operator | 自建 | 见本仓库 `skills/_implementation-details/multica-cli-operator/` |
| review-acceptance-operator | 自建 | 无公开下载 |

> **说明**：
> - ClawHub 公开 skill **不上传文件**，仅提供安装链接
> - "自建" skill 是作者根据自身工作流定制的，不建议直接复用
> - `cn-skill-router` 和 `multica-cli-operator` 的参考实现在本仓库 `skills/_implementation-details/` 中
> - `Bookmark Keeper` 和 `Openai Image Gen` 来源待确认，如你知道确切来源请补充
