# 安全说明

## 本项目已脱敏

本项目作为开源模板发布，已经过以下处理：

- 不包含任何 API key、token、cookie、私钥
- 不包含真实本地绝对路径（全部使用占位符）
- 不包含真实 workspace 名称或 slug
- 不包含真实 issue ID
- 不包含邮箱或真实账号名
- 不包含模型账单或供应商密钥信息

## 使用本模板时的安全注意事项

如果你基于本模板创建自己的项目并计划公开发布：

1. **运行脱敏检查**：逐项检查个人信息、密钥、路径、workspace 信息和内部链接
2. **不要上传 `.multica/` 目录内容**
3. **不要上传 `daemon.log` 或其他日志文件**
4. **检查 issue description 模板中是否有真实数据**
5. **用占位符替换所有个人化信息**
6. **只发布抽象方法、模板和示例，不发布原始运行记录**
7. **发布前执行 `docs/examples/release-gate-checklist.zh.md` 中的检查**

## Agent 安全

- 高权限 skill（Win Mouse Native、Pre Publish Security、Aj Github 等）分配前需确认必要性
- 不要让 agent 自动执行 git push、发布、支付等操作
- 创建 manager agent 时，在其 instructions 中明确禁止操作列表
- 对中大型任务，建议先写 ContextPacket / TaskContract，再派给 worker agent
- Agent 声称完成时，应返回 changed paths、verification、assumptions、blockers 和 next action

## 第三方 Skill 安全提醒

本项目文档中引用的 ClawHub/Anthropic 第三方 skill **不包含在本仓库中**，仅以链接形式提供参考。

安装任何第三方 skill 前，建议：
1. 查看 skill 源码（SKILL.md 和关联脚本）
2. 检查 skill 的权限声明和网络请求
3. 先在不含敏感数据的测试环境中试用
4. 已安装的 skill 建议定期审查和更新

本仓库内的 `skills/_implementation-details/` 目录中的参考实现已经过脱敏处理，可直接安全使用。

## 报告安全问题

如果你在本项目中发现了安全问题（例如遗漏的个人信息），请通过 GitHub Issues 报告。
