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

## Agent 安全

- 高权限 skill（Win Mouse Native、Pre Publish Security、Aj Github 等）分配前需确认必要性
- 不要让 agent 自动执行 git push、发布、支付等操作
- 创建 manager agent 时，在其 instructions 中明确禁止操作列表

## 报告安全问题

如果你在本项目中发现了安全问题（例如遗漏的个人信息），请通过 GitHub Issues 报告。
