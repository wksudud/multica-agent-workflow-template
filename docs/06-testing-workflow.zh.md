# 06 — 测试流程

> 在开始真实任务之前，必须逐层测试。每层通过后再进入下一层。

---

## 第 0 层：环境健康检查

```bash
# 1. CLI 可用？
multica version

# 2. 认证状态
multica daemon status

# 3. Agent 列表
multica agent list --output json

# 4. Workspace 信息
multica workspace get --output json
```

预期：所有命令正常返回，agent 列表非空。

---

## 第 1 层：写作轻任务测试

**目的**：验证最简单的 agent 能否正常接收和执行任务。

**任务**：让 Writing-Light（或你的轻量 writer）写一段 200 字以内的自我介绍。

**创建 issue**：
- 标题：`[TEST] 测试 <agent名称> 是否能正常接收任务`
- Assignee：你的轻量 writer agent
- 描述：要求写一段简短自我介绍，不修改文件

**验证标准**：
- [ ] Issue 创建成功
- [ ] Agent 自动接收
- [ ] Agent 在 issue 中回复了内容
- [ ] 内容符合要求
- [ ] 没有修改任何文件

---

## 第 2 层：编程分诊测试

**目的**：验证轻量 agent 能否正确判断任务复杂度。

**任务**：让 Coding-Light（或你的分诊 agent）判断一个中等难度编程任务应该交给谁。

**创建 issue**：
- 标题：`[TEST] 测试 <agent名称> 的任务分诊能力`
- Assignee：你的分诊 agent
- 描述：给出一个具体的编程任务，要求判断复杂度、推荐 agent 和 skill

**验证标准**：
- [ ] Agent 正确判断了复杂度
- [ ] Agent 推荐了合适的 worker agent
- [ ] Agent 推荐了合理的 skill
- [ ] Agent 说明了为什么不需要最强模型（如适用）
- [ ] 推荐结果与你的默认规则一致

---

## 第 3 层：Router 判断测试

**目的**：验证 skill router 能否正确分类多种任务。

**任务**：向 manager agent（或直接使用 cn-skill-router）提问 5 种不同任务，看路由是否正确。

**任务类型**：
1. 编程修复类（中等）
2. 写作正文类（中等）
3. 最终审查类（高难）
4. 资料整理类（简单）
5. 新项目创建类（中等）

**验证标准**：
- [ ] 所有 5 个任务的 agent 推荐都合理
- [ ] 简单任务没有推荐最强模型
- [ ] 高难任务正确推荐了最强模型
- [ ] Skill 推荐在 1-3 个范围内

---

## 第 4 层：Router + CLI 真实派单测试

**目的**：验证路由判断 → issue 创建 → agent 执行 → 结果汇总的完整链路。

**任务**：一个真实的短文写作任务（如 300 字中文说明）。

**验证标准**：
- [ ] cn-skill-router 正确推荐了 agent 和 skill
- [ ] multica-cli-operator 成功创建 issue
- [ ] Agent 成功执行并回复
- [ ] Manager 成功读取并汇总结果
- [ ] 全链路无报错

---

## 第 5 层：Pro / Review 层真实任务测试

**目的**：验证主力 agent 和审查 agent。

**任务 A（Pro 层）**：一个真实的代码修复或论文写作任务。
**任务 B（Review 层，可选）**：对任务 A 的结果进行审查。

**验证标准**：
- [ ] Pro agent 正确执行了任务
- [ ] 输出质量远高于 Light agent（如适用）
- [ ] Review agent 提出了有价值的审查意见（如适用）

---

## 测试通过标准

所有 0-4 层测试通过后，可以开始真实任务。

第 5 层（Pro/Review）可以在真实任务中验证。

---

## 测试后清理

测试完成后，将测试 issue 的状态更新为 done 或 cancelled：

```bash
multica issue status <test-issue-id> done
```
