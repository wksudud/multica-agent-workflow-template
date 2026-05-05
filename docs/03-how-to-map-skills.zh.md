# 03 — 如何映射 Skill 到 Agent

> 本指南帮助你决定每个 skill 应该分配给哪些 agent。

---

## 核心原则

1. **不要每个 agent 都开启全部 skill**。全部开启 = 没有分工。
2. **一个 skill 可以分配给多个 agent**（如 karpathy-guidelines 适合所有编程 agent）。
3. **高权限 skill 谨慎分配**（只给最需要它的 agent）。
4. **路由和 CLI 类 skill 只给 manager agent**。

---

## Skill 分类和分配建议

### 路由与调度类

| Skill | 典型用途 | 建议分配给 |
|-------|---------|-----------|
| cn-skill-router | 中文任务分类和 skill 推荐 | **Manager only** |
| multica-cli-operator | 创建 issue、分配 agent、查看 runs | **Manager only** |

### 编程类

| Skill | 典型用途 | 建议分配给 |
|-------|---------|-----------|
| Coding Lead | 编程执行流程规范 | 所有 Coding agent |
| karpathy-guidelines | 减少编码错误，最小必要修改 | 所有 Coding agent |
| Agent Git Oracle | 代码库分析、技术债识别 | Coding-Main、Coding-Review |
| OpenClaw JSON Toolkit | JSON 格式化、校验 | Coding-Light、Coding-Main |
| SpecVibe | 新项目需求规格 | Coding-Main |
| Aj Github | GitHub 操作 | Coding-Main、Coding-Review |

### 写作类

| Skill | 典型用途 | 建议分配给 |
|-------|---------|-----------|
| doc-coauthoring | 结构化文档写作 | 所有 Writing agent |
| Humanizer | AI 文本自然化 | Writing-Main、Writing-Review |
| 深度话题调研工作流 | 技术综述、背景调研 | Writing-Light、Writing-Main |
| Obsidian Markdown | Obsidian 笔记 | Writing-Light |
| Bookmark Keeper | 网页链接管理 | Writing-Light |
| Codebase Documenter | 代码库文档生成 | Writing-Main（或 Coding-Main） |

### 审查与安全类

| Skill | 典型用途 | 建议分配给 |
|-------|---------|-----------|
| Pre Publish Security | 发布前安全检查 | Coding-Review、Review agent |
| skill-guard | Skill 安装前安全扫描 | Manager、Review agent |
| security-review | 安全审查 | Coding-Review、Review agent |

### 系统与高权限类

| Skill | 典型用途 | 建议分配给 | 风险 |
|-------|---------|-----------|------|
| Win Mouse Native (Windows) | Windows 鼠标控制 | **不分配给普通 agent** | 高：能控制桌面 |

---

## 映射示例

### 示例：3-agent 能力分层 + skill 分配

| Agent | 模型 | 分配的 Skills |
|-------|------|-------------|
| Light | Flash | karpathy-guidelines, OpenClaw JSON Toolkit, Obsidian Markdown, Bookmark Keeper |
| Main | Pro | Coding Lead, karpathy-guidelines, doc-coauthoring, Humanizer, Codebase Documenter, Agent Git Oracle |
| Review | GPT/Opus | karpathy-guidelines, Agent Git Oracle, Pre Publish Security, security-review |

### 示例：写作 + 编程双线 × 三层

见 `examples/skill-mapping-examples.zh.md`。

---

## 自检清单

在分配 skill 之后，用这个清单验证：

- [ ] 每个 agent 的 skill 数量在 3-10 之间（太多会增加判断负担）
- [ ] Manager agent 有 cn-skill-router 和 multica-cli-operator
- [ ] 高权限 skill 只分配给明确需要的 agent
- [ ] 审查类 skill 分配给了 Review agent
- [ ] 没有无脑把所有 skill 全开给所有 agent
