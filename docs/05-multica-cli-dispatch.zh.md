# 05 — Multica CLI 派单说明

## 核心概念

**Multica 的 agent 调度是 issue-based coordination，不是同步函数调用。**

这意味着：
- Agent A 不能像调用函数一样直接"调用" Agent B 并立刻拿到返回值
- 稳定的协作方式是通过 issue 系统：
  1. Manager 创建 issue
  2. 分配 assignee
  3. Worker agent 接收并执行
  4. Worker 在 issue comment 中回复结果
  5. Manager 通过 runs 和 run-messages 读取结果

---

## 关键命令

### 读取类（可以直接执行）

```bash
# 检查 CLI 版本
multica version

# 检查 daemon 状态
multica daemon status

# 查看 agent 列表
multica agent list --output json

# 查看 issue 列表
multica issue list --output json

# 查看 issue 详情
multica issue get <issue-id> --output json

# 查看 issue 的 runs
multica issue runs <issue-id> --output json

# 查看 run 的消息
multica issue run-messages <task-id> --output json

# 查看 issue 评论
multica issue comment list <issue-id> --output json
```

### 写入类（需确认用户意图）

```bash
# 创建 issue
multica issue create --title "..." --assignee "..." --description-stdin --output json

# 分配 issue
multica issue assign <issue-id> --to "agent-name"

# 更新 issue 状态
multica issue status <issue-id> in_progress

# 添加评论
cat <<'EOF' | multica issue comment add <issue-id> --content-stdin
评论内容
EOF
```

---

## 自发现流程

CLI 版本会更新，参数可能变化。执行前先看 help：

```bash
multica issue create --help
multica issue assign --help
multica issue runs --help
multica issue run-messages --help
```

**不要凭空假设参数存在。**

---

## 典型派单流程

```
1. 加载 cn-skill-router → 判断任务类型
2. 加载 multica-cli-operator → 准备 issue
3. multica issue create → 创建 issue + 分配 agent
4. 等待 agent 执行
5. multica issue runs <id> → 查看运行状态
6. multica issue run-messages <task-id> → 读取详细输出
7. 汇总结果 → 反馈用户
```

---

## Issue Description 模板

每个 issue 的 description 应包含：

1. **目标** — 一句话说明要完成什么
2. **背景** — 为什么需要这个任务
3. **相关文件** — 需要读取或修改的文件路径
4. **推荐 agent** — 目标 agent 完整名称
5. **推荐 skill** — 1-3 个最相关的 skill
6. **限制条件** — 不能做什么
7. **期望输出** — 完成后的产物
8. **验证方式** — 如何确认已完成且正确
9. **不要做** — 明确列出禁止操作

详细模板见 `examples/issue-description-templates.zh.md`。
