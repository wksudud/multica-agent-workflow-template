# 04 — 创建 multica-cli-operator Skill

> 将此提示词发送给你的 AI agent，创建 CLI 操作 skill。

---

请帮我创建一个新的 skill：`multica-cli-operator`

## Skill 用途

让 Multica 中的 AI agent 能够安全、稳定、可验证地使用 `multica` CLI 来：
- 读取 agent 列表
- 检查 daemon 状态
- 创建 issue 并分配 agent
- 查看 issue 的 runs 和 run messages
- 汇总其他 agent 的执行结果

## 内容要求

1. 使用 YAML frontmatter（name + description）
2. 说明 Multica 的 agent 调度是 issue-based coordination，不是同步函数调用
3. 安全规则：读取命令可以直接执行，写入命令需确认用户意图
4. CLI 自发现流程：执行前先 `--help`，不同版本参数可能不同
5. 基础检查：version、daemon status、workspace get
6. Agent 读取：agent list → 整理为表格
7. Issue 创建：description 必须包含目标、背景、文件路径、限制条件、期望输出
8. Issue 分配：使用完整 agent 名称
9. Runs 查看：runs + run-messages
10. 错误处理：覆盖常见错误和解决建议
11. 禁止事项：不删除、不自动提交、不泄露密钥、不循环派单

## 安全要求

- 不要安装依赖
- 不要修改系统配置
- 不要覆盖已有 skill
- 所有命令基于实际 CLI 版本，先 `--help` 再执行

## 参考

可以基于 `skills/multica-cli-operator/SKILL.md` 创建，但需要根据你的实际 CLI 版本调整命令。
