---
name: multica-workflow-bootstrapper
description: >
  Multica 工作流初始化向导。
  Use when the user wants to design or bootstrap a personalized
  Multica multi-agent + skill workflow based on their models,
  costs, quotas, installed skills, and task preferences.
---

# Multica Workflow Bootstrapper

> 这是 Multica 多 agent + skill 工作流的**初始化配置向导**。
> 它用于"从零开始规划和初始化"。日常派单不归它管。

---

## Purpose / 用途

multica-workflow-bootstrapper 用于：

1. 引导用户描述自己的模型配置、成本和额度限制
2. 读取当前 Multica agent 列表和本地 skills 列表
3. 根据用户需求推荐 agent 数量和结构（不是固定配置）
4. 推荐 agent 命名、职责、skill 分配
5. 生成每个 agent 的完整指令模板
6. 判断是否需要 Manager agent、cn-skill-router、multica-cli-operator
7. 可选：从用户指定的 GitHub 仓库或本地路径复制模板 skill
8. 生成低风险测试流程和发布前安全检查清单

**这个 skill 不负责日常派单。** 日常派单由 `cn-skill-router` + `multica-cli-operator` 完成。

---

## When to Use / 何时使用

**适合：**
- 第一次搭建 Multica 多 agent 工作流
- 想根据模型/成本/额度设计 agent 结构
- 不确定应该创建几个 agent
- 想复刻某个模板但需要根据自己的情况调整
- 想为团队或项目生成标准化的 agent 配置

**不适合：**
- 日常任务路由（用 cn-skill-router）
- 创建 issue 或分配 agent（用 multica-cli-operator）
- 修改已有 agent 的单个配置
- 修复单个 bug 或写作任务

---

## Safety Rules / 安全规则

1. **不要求用户输入真实 API key。** 只需要模型能力、价格、额度的描述
2. **不把 token、cookie、私钥写进 issue 或配置文件**
3. **不自动下载未知代码**
4. **不自动执行安装脚本（npm install / pip install / curl | sh）**
5. **不自动 push 到 GitHub**
6. **不自动创建远程仓库**
7. **不删除用户已有 skill**
8. **高权限 skill 必须标记风险**
9. **最强模型只用于高价值任务**
10. **所有配置建议基于用户实际存在的模型和 skill**
11. **如果信息不足，先给最小可用配置，再列出可选升级方案**

---

## Six Working Modes / 六种工作模式

---

### Mode A：需求访谈模式

**触发条件：** 用户说"我想搭建 Multica 多 agent 工作流"、"帮我设计 agent"、"我不确定应该创建几个 agent"等。

**步骤：** 引导用户回答以下问题（不需要全部一次问完，根据对话自然推进）：

| # | 问题 | 类别 |
|---|------|------|
| 1 | 你有哪些模型？名称和能力评级？ | 模型 |
| 2 | 每个模型的成本如何？（高/中/低，或具体价格） | 成本 |
| 3 | 哪些模型有额度限制？ | 额度 |
| 4 | 你主要做写作、编程、研究、自动化还是混合任务？ | 任务类型 |
| 5 | 你已经安装了哪些 skill？ | Skills |
| 6 | 你希望 agent 数量少还是分工细？ | 偏好 |
| 7 | 你是否需要一个总调度 agent（manager）？ | 架构 |
| 8 | 哪些任务必须用最强模型？ | 优先级 |
| 9 | 哪些任务可以用便宜模型？ | 优先级 |
| 10 | 是否需要 GitHub 操作？ | 权限 |
| 11 | 是否需要 Windows/浏览器/本地文件操作？ | 权限 |
| 12 | 是否希望自动创建 issue 和派单？ | 自动化 |

**输出：** 整理成"用户配置理解"摘要。

---

### Mode B：本地扫描模式

**触发条件：** 用户说"扫描我当前的 agent 和 skill"、"读取当前配置"等。

**步骤：**

1. 使用 `multica agent list --output json` 读取当前 agent 列表
2. 扫描本地 skills 目录（`.claude/skills/`、`~/.claude/skills/`）
3. 查找所有 `SKILL.md` 文件
4. 提取每个 skill 的 frontmatter（name、description）
5. 标记高权限 skill（Win Mouse Native、Pre Publish Security、Aj Github 等）
6. 标记用途不清楚的 skill
7. 输出当前状态

**输出格式：**

```
## 当前 Agent 列表
| Agent | 模型 | 状态 | Skills |

## 当前本地 Skills
| Skill | 类型 | 风险等级 | 确定性 |

## 需要确认的 Skill
（用途不清楚或高权限的 skill）
```

**注意：** 只读扫描可以直接执行。写入文件或下载前必须有用户明确要求。

---

### Mode C：配置推荐模式

**触发条件：** 访谈和扫描完成后，用户说"给我推荐方案"。

**推荐以下方案之一（或用户自定义组合）：**

#### 方案 A：极简 2-agent
| Agent | 职责 | 推荐模型 |
|-------|------|---------|
| Manager | 任务路由、创建 issue、汇总结果 | 主力模型 |
| Worker | 执行所有任务 | 主力模型 |

适合：skill < 5、任务简单、不想维护太多 agent

#### 方案 B：三层 3-agent
| Agent | 职责 | 推荐模型 |
|-------|------|---------|
| Light | 轻任务、分诊 | 经济模型 |
| Main | 主力执行 | 主力模型 |
| Review | 高质量审查 | 最强模型 |

适合：模型有明显能力和成本差异、想节省最强模型额度

#### 方案 C：按领域拆分 4-agent
| Agent | 职责 |
|-------|------|
| Writing Agent | 写作、润色、翻译 |
| Coding Agent | 编程、调试、文档 |
| Research Agent | 调研、分析 |
| Review Agent | 质量审查 |

适合：任务领域差异明显

#### 方案 D：双线三层 6-agent
写作线：Writing-Light / Writing-Main / Writing-Review
编程线：Coding-Light / Coding-Main / Coding-Review

适合：写作和编程任务都很多、追求精细分工

#### 方案 E：用户自定义
根据用户需求自由组合。例如：
- 2-agent（不区分写作编程）+ 1 个 Review
- 3-agent 按成本分层（不区分写作编程）
- 5-agent（2 写作 + 2 编程 + 1 Manager）

#### 方案 F：作者案例（Example Case Study）
1 Manager + 3 Coding + 3 Writing = 7 agent，双线 × 三层。

**必须说明：作者案例只是 Example Case Study，不是标准配置。**

---

### Mode D：生成 Agent 指令模式

**触发条件：** 方案确定后，用户说"生成 agent 指令"。

**为每个 agent 生成：**

1. Agent 名称（中文 + 英文可选）
2. 底层模型
3. 定位（一句话）
4. 适合任务
5. 不适合任务
6. 推荐开启 skill
7. 不建议开启 skill
8. 完整指令模板

**输出格式：**

```
## Agent 配置总览

| Agent | 模型 | 定位 | 推荐 Skill | 不建议 Skill |
|-------|------|------|-----------|-------------|

## <Agent 1 名称> 指令

（完整的 agent instructions，可直接复制到 Multica agent 配置中）

## <Agent 2 名称> 指令

...
```

---

### Mode E：模板安装 / 复制模式

**触发条件：** 用户明确说"下载模板到本地"、"安装模板 skill"、"复制这个 skill"等。

**允许操作：**
1. 从用户指定的 GitHub 仓库下载模板文件
2. 从用户指定的本地路径复制模板
3. 写入 `.claude/skills/` 下的指定 skill 目录
4. 创建 cn-skill-router / multica-cli-operator / multica-workflow-bootstrapper

**安全要求：**
1. 下载前必须展示来源 URL
2. 下载前必须展示目标路径
3. 只复制明确需要的文件
4. 不执行任何下载来的脚本
5. 不运行 `npm install` / `pip install` / `curl | sh`
6. 不修改系统配置
7. 不删除已有 skill
8. 如果同名 skill 已存在，先备份或提示用户
9. 下载后使用 skill-guard 或安全清单检查
10. 发现可疑内容时停止

---

### Mode F：低风险测试模式

**触发条件：** agent 创建完成后，用户说"测试我的工作流"。

**生成并按顺序执行测试：**

| 层 | 测试内容 | 验证标准 |
|----|---------|---------|
| 0 | 健康检查（version, daemon, agent list） | CLI 正常，agent 全部 online |
| 1 | 写作轻任务：派给最轻量 writer，写短文 | Agent 回复，未修改文件 |
| 2 | 编程分诊：派给最轻量 coder，判断任务 | 复杂度判断合理，推荐正确 |
| 3 | Router 判断：用 cn-skill-router 判断 5 种任务 | 全部推荐合理 |
| 4 | Router + CLI 完整链路：真实派单 | 创建→执行→汇总，全链路无报错 |
| 5 | 主力 Agent：Pro/Main 执行真实但低风险任务 | 质量明显高于 Light |
| 6 | Review Agent：仅在真实审查需求时测试 | 不浪费高额度模型 |

每层通过后再进入下一层。详细流程见 `multica-agent-workflow-template/docs/06-testing-workflow.zh.md`。

---

## Output Templates / 输出模板

### 用户配置理解
```
## 用户配置理解

| 维度 | 详情 |
|------|------|
| 模型 | <模型列表 + 能力评级> |
| 成本 | <高/中/低 分布> |
| 额度限制 | <哪些模型有限额> |
| 任务类型 | <写作/编程/混合> |
| 已安装 Skills | <数量 + 关键 skill> |
| 偏好 | <少/细/有 Manager/无 Manager> |
```

### 推荐 Agent 结构
```
## 推荐 Agent 结构

推荐<方案 X>：<N> 个 agent。

理由：<1-3 条>

| Agent | 模型 | 定位 | 适合 | 不适合 |
|-------|------|------|------|--------|
```

### 推荐 Skill 分配
```
## 推荐 Skill 分配

| Agent | 推荐 Skill | 原因 |
|-------|-----------|------|
```

### 需要创建的辅助 Skill
```
## 辅助 Skill

| Skill | 是否需要 | 原因 |
|-------|---------|------|
| cn-skill-router | 是/否 | <原因> |
| multica-cli-operator | 是/否 | <原因> |
| multica-workflow-bootstrapper | 是/否 | <原因> |
```

---

## 职责边界

| Skill | 负责 | 不负责 |
|-------|------|--------|
| **multica-workflow-bootstrapper** | 初始化规划、生成配置建议、生成 agent 指令、指导安装 | 日常派单、创建 issue、任务路由 |
| **cn-skill-router** | 日常中文任务路由、判断任务该交给谁、推荐 skill | 初始化配置、生成 agent 指令 |
| **multica-cli-operator** | 实际操作 Multica CLI、创建 issue、分配 agent、读取 runs | 任务分类、skill 推荐、配置规划 |

**三者不互相替代。** 初始化用 bootstrapper，日常路由用 cn-skill-router，CLI 操作用 multica-cli-operator。

---

## Quick Reference / 速查

| 用户说什么 | 进入哪个模式 | 做什么 |
|-----------|------------|--------|
| "我想搭建工作流" | Mode A 访谈 | 了解模型/skill/偏好 |
| "扫描我当前的配置" | Mode B 扫描 | 读 agent list + skill 目录 |
| "给我推荐方案" | Mode C 推荐 | 推荐 A-F 方案之一 |
| "生成 agent 指令" | Mode D 指令 | 输出每个 agent 的完整配置 |
| "下载/安装模板" | Mode E 安装 | 安全检查后复制文件 |
| "测试我的工作流" | Mode F 测试 | 按 0-6 层测试流程执行 |
