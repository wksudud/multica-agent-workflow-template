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

1. **运行脱敏检查**：见 `docs/09-sanitization-checklist.zh.md`
2. **不要上传 `.multica/` 目录内容**
3. **不要上传 `daemon.log` 或其他日志文件**
4. **检查 issue description 模板中是否有真实数据**
5. **用占位符替换所有个人化信息**

## 发布流程

将本模板发布到 GitHub 时，必须遵循以下流程：

1. **先上传到 private 仓库**（不要直接创建 public 仓库）
2. **在 GitHub 上检查文件视图**：逐文件确认无敏感信息暴露
3. **启用 GitHub secret scanning**：让 GitHub 自动扫描可能的密钥和令牌
4. **重新运行脱敏检查**：`docs/09-sanitization-checklist.zh.md`
5. **人工确认**：以上全部通过后，由人类明确决定是否可以公开
6. **转为 public**：确认后方可将仓库可见性改为 public

如果项目是从已有 git 历史的仓库发布，还需要运行 git history secret scanning 检查历史提交中是否包含敏感信息。

## Agent 安全

- 高权限 skill（Win Mouse Native、Pre Publish Security、Aj Github 等）分配前需确认必要性
- 不要让 agent 自动执行 git push、发布、支付等操作
- 创建 manager agent 时，在其 instructions 中明确禁止操作列表

## 报告安全问题

如果你在本项目中发现了安全问题（例如遗漏的个人信息），请通过 GitHub Issues 报告。
