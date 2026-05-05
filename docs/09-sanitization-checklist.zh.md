# 09 — 开源前脱敏检查清单

> 在将你的项目发布到 GitHub 或其他公开平台前，逐项检查以下内容。

---

## 个人信息

- [ ] 邮箱地址（包括 agent instructions 中的）
- [ ] 真实姓名
- [ ] 账号名 / 用户名
- [ ] 手机号
- [ ] 社交媒体账号

## 认证与密钥

- [ ] API key（任何服务商的）
- [ ] Token / access token
- [ ] Cookie / session
- [ ] 私钥（.pem, .key 文件）
- [ ] 密码（包括注释中的）
- [ ] OAuth client secret

## Multica 特定

- [ ] Workspace ID
- [ ] Workspace slug / name
- [ ] Issue ID（真实 issue 的 UUID）
- [ ] Agent ID（真实 agent 的 UUID）
- [ ] Server URL（如有自定义）
- [ ] Daemon ID 或 device name

## 路径

- [ ] 本地绝对路径（如 `C:\Users\...` 或 `/home/...`）
- [ ] 项目真实路径
- [ ] 日志文件路径
- [ ] 配置文件路径（含用户名的）

## 项目特定

- [ ] 真实项目名（如果不想暴露）
- [ ] 模型账单信息
- [ ] 供应商密钥
- [ ] 内部文档链接
- [ ] 私有仓库 URL

## 替换为占位符

以上信息如果出现在文件中，应替换为通用占位符：

| 信息 | 替换为 |
|------|--------|
| 用户名 | `<用户名>` |
| workspace slug | `<your-workspace>` |
| issue ID | `<issue-id>` |
| agent ID | `<agent-id>` |
| 绝对路径 | `<项目根目录>/...` |
| 邮箱 | `<your-email>` |

---

## 自动化检查

如果有 Pre Publish Security skill，可在发布前运行：

```
使用 Pre Publish Security 扫描当前项目目录
```

---

## 最后提醒

**脱敏不是一次性的。** 每次添加新文件或更新内容后，都应该重新运行这个检查清单。
