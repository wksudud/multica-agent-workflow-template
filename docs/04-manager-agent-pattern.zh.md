# 04 — Manager Agent 模式

## 什么是 Manager Agent？

Manager agent 是一个"不执行具体任务，只负责任务调度"的 agent。它：

- 读取当前 agent 配置（谁在线、什么能力）
- 理解用户的中文任务描述
- 判断任务类型和复杂度
- 推荐最合适的 worker agent
- 推荐 1-3 个最合适的 skill
- 通过 multica CLI 创建 issue 并分配给 worker
- 等待 worker 完成后读取结果
- 汇总结果反馈给用户

---

## 什么时候需要 Manager Agent？

| 情况 | 是否需要 Manager |
|------|-----------------|
| 只有 1-2 个 agent | **不需要**，手动分配即可 |
| 有 3-4 个 agent，任务类型单一 | **可选**，有 Manager 更省心 |
| 有 5+ 个 agent，任务类型多样 | **强烈建议**，否则手动判断太累 |
| 想实现"用户说中文 → 自动判断 agent" | **需要** |

**结论：小规模用户不一定需要 Manager Agent。**

---

## Manager Agent 的核心指令模板

创建 manager agent 时需要包含以下要点（用占位符）：

```
你是 <Manager名称>，负责：
1. 读取当前 Multica agent / skill 配置
2. 根据用户任务判断应该交给哪个 agent
3. 根据任务类型选择合适 skill
4. 通过 Multica CLI / issue 系统创建、分配、跟踪任务
5. 汇总其他 agent 的结果并反馈给用户

核心原则：
- 你不是所有任务都自己做
- 简单任务最多选 1 个 skill
- 中等任务最多选 2 个 skill
- 复杂任务最多选 3 个 skill
- 优先把真正执行任务交给更合适的专门 agent
- 不要假设可以同步调用另一个 agent
- Multica 的稳定协作单位是 issue、assignee、run、comment

默认路由规则：
<根据你的实际 agent 结构填写>
```

见 `prompts/03-create-manager-agent.zh.md` 中的完整模板。

---

## Manager Agent 应该开启的 Skills

| Skill | 用途 |
|-------|------|
| cn-skill-router | 任务分类、agent 推荐、skill 推荐 |
| multica-cli-operator | 实际 CLI 操作（创建 issue、分配 agent） |

可选：skill-guard（如果 manager 也负责安装 skill 前的安全检查）。

---

## Manager Agent 不应该做的事

- 不应该抢 worker 的活（如果自己能做就不分派，manager 就失去了意义）
- 不应该假设可以"同步调用"其他 agent
- 不应该创建循环派单
- 不应该在用户说"看看"时就创建 issue

---

## 没有 Manager 时的替代方案

如果不想创建 manager agent，有两种替代方案：

### 方案 1：手动判断 + 手动分配
用户自己判断任务类型，然后手动创建 issue 并分配。

### 方案 2：用聊天窗口临时决策
直接用 Multica 的聊天功能，向一个 agent 描述任务，让它给出建议（但不执行）。
