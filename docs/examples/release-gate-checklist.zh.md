# 发布前检查清单

用于把个人 Multica 工作流、agent 配置经验或项目模板发布到 GitHub 前做最后检查。

---

## 1. 内容边界

- [ ] 没有真实 workspace ID
- [ ] 没有真实 agent ID / skill ID / runtime ID
- [ ] 没有真实 issue ID / run ID / comment ID
- [ ] 没有本地绝对路径
- [ ] 没有用户名、邮箱、账号名等个人信息
- [ ] 没有 daemon.log、runtime ledger、原始运行日志
- [ ] 没有真实业务数据、个人文档、简历、论文或客户材料

---

## 2. 凭据边界

- [ ] 没有 token
- [ ] 没有 cookie
- [ ] 没有 API key
- [ ] 没有私钥
- [ ] 没有 `.env`
- [ ] 没有模型供应商账单、额度、账号信息

---

## 3. 操作边界

- [ ] 文档没有鼓励 agent 自动 push
- [ ] 文档没有鼓励 agent 自动删除文件或仓库
- [ ] 文档没有鼓励 agent 自动修改账号权限
- [ ] 文档没有鼓励 agent 自动付款或调用付费高风险接口
- [ ] 高权限 skill 已标注风险

---

## 4. 模板质量

- [ ] 示例使用占位符
- [ ] 示例不是作者机器上的真实配置
- [ ] 说明了"不要照抄，要按自己的模型、预算和任务类型调整"
- [ ] 有最小可用版本
- [ ] 有测试流程
- [ ] 有失败时的停止条件

---

## 5. 建议扫描命令

```bash
# 常见私密标记
rg -n "token|api[_-]?key|secret|cookie|private key|BEGIN .* KEY|\\.env" .

# 常见本地路径
rg -n "C:\\\\|/Users/|/home/|workspace_id|agent_id|skill_id|runtime_id|issue_id|run_id" .

# Git 状态
git status --short
git diff --check
```

这些命令不能替代人工审查，但能帮你发现明显问题。

