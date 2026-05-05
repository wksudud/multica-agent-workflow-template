# 06 — 测试 Multica 派单

> 将以下提示词发送给 manager agent，进行低风险派单测试。

---

请进行 Multica 派单测试。要求：

## 测试 1：健康检查

```bash
multica version
multica daemon status
multica agent list --output json
multica workspace get --output json
```

确认 CLI 正常、agent 列表非空。

## 测试 2：写作轻任务派单

创建一个 issue，分配给写作轻任务 agent：

- 标题：`[TEST] 测试 <agent名称> 是否能正常接收任务`
- 内容：写一段 200 字以内的自我介绍
- 验证：agent 在 issue 中回复了内容，未修改文件

## 测试 3：编程分诊测试

创建一个 issue，分配给编程分诊 agent：

- 标题：`[TEST] 测试 <agent名称> 的任务分诊能力`
- 内容：给出一个中等难度的编程任务，要求判断复杂度、推荐 agent 和 skill
- 验证：推荐结果合理，说明了为什么不需要最强模型

## 测试 4：路由判断测试

用 cn-skill-router 判断 5 个不同任务类型（编程修复、论文写作、最终审查、资料整理、新项目创建）。

验证：
- 简单任务没推荐最强模型
- 高难任务正确推荐了最强模型
- Skill 推荐在 1-3 个范围内

## 测试 5：完整链路测试

完成一个真实派单：路由判断 → 创建 issue → agent 执行 → 结果汇总。

验证全链路无报错。

---

以上测试全部通过后，可以开始真实任务派单。
