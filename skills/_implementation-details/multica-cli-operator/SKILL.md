---
name: multica-cli-operator
description: Multica CLI 操作技能。Use when an agent needs to inspect Multica agents, create or assign issues, check daemon status, list runs, read run messages, or coordinate other agents through the multica CLI.
---

# Multica CLI Operator

> 本文档写给 AI agent 阅读，不是给普通用户的教程。
> 核心原则：**Multica 的 agent 调度是 issue-based coordination，不是同步函数调用。**

---

## Purpose / 用途

本 skill 用于指导 agent 安全、稳定地使用 `multica` CLI 完成以下操作：

- 读取 Multica agent 列表
- 检查 daemon 状态
- 创建 issue（附带完整 description）
- 分配 issue（assign）
- 查看 issue 列表（支持筛选）
- 查看 issue 的执行记录（runs）
- 读取某次运行的详细消息（run-messages）
- 汇总其他 agent 的执行结果

明确：**本 skill 不提供 agent 间同步调用的方法。** 所有跨 agent 协调必须通过 issue 系统完成。

---

## When to Use / 何时使用

**适合**：读取 agent 配置、创建 issue、分配任务、查看进度、汇总结果、检查 daemon 状态。

**不适合**：普通文本润色、代码解释、不需要 Multica 的单轮问答、用户未授权创建 issue 的情况。

---

## Safety Rules / 安全规则

1. **读取状态类命令可以直接执行**（agent list, issue list, issue get, daemon status, workspace get）。
2. **创建 issue 或分配任务前，用户必须有明确意图。**
3. **删除、关闭、覆盖、发布、提交、推送等高风险操作必须二次确认。**
4. **不要把 API key、token、cookie、私钥、密码写入 issue description。**
5. **不要创建无限循环任务或让 agent 互相反复派单。**
6. **不要假装命令成功。** CLI 报错必须如实反馈。
7. **如果命令参数不确定，先运行 `--help`。**
8. **不要用 `curl`/`wget` 直接调用 Multica API。** 必须通过 `multica` CLI。

---

## CLI 自发现流程

不同版本的 `multica` CLI 参数可能不同。执行前始终先看 help：

```bash
multica --help
multica agent --help
multica issue --help
multica daemon --help

# 子命令
multica issue create --help
multica issue assign --help
multica issue list --help
multica issue runs --help
multica issue run-messages --help
```

**永远不要凭空假设参数存在。**

---

## 基础检查

```bash
multica version
multica daemon status
multica workspace get --output json
```

---

## 读取 Agent

```bash
multica agent list --output json
```

整理为表格：| Agent | Runtime | Status | Notes |

---

## 创建 Issue

Issue description 必须包含：目标、背景、相关文件、推荐 agent、推荐 skill、限制条件、期望输出、验证方式、不要做。

```bash
cat <<'EOF' | multica issue create \
  --title "[类型] 简短任务名" \
  --priority medium \
  --assignee "目标agent名称" \
  --description-stdin \
  --output json
目标：...
背景：...
相关文件：...
推荐 agent：...
推荐 skill：...
限制条件：...
期望输出：...
验证方式：...
不要做：...
EOF
```

---

## 分配 Issue

```bash
multica issue assign <issue-id> --to "agent完整名称" --output json
```

分配前先读取 agent list 确认名称。如果名称有歧义，列出候选项要求确认。

---

## 查看 Runs 和 Messages

```bash
multica issue runs <issue-id> --output json
multica issue run-messages <task-id> --output json
```

汇总格式：
```
## 执行状态：完成 / 失败 / 阻塞 / 进行中
## 关键输出：...
## 文件改动：...
## 阻塞原因：...
## 下一步建议：...
```

---

## 错误处理

| 错误 | 解释 | 下一步 |
|------|------|--------|
| command not found | multica 未安装/不在 PATH | 检查安装和 PATH |
| daemon not running | daemon 未启动 | `multica daemon start` |
| assignee not found | agent 名称不存在 | 运行 `multica agent list` 确认名称 |
| run failed | agent 执行失败 | 查看 run messages，考虑升级 agent |

---

## 禁止事项

- 不要删除 issue / workspace / agent / skill
- 不要自动 git commit / push / 发布
- 不要在 issue 中泄露密钥
- 不要创建循环派单
- 不要假设 agent 间可以同步调用
- 不要在没看 `--help` 时强行使用不确定参数
