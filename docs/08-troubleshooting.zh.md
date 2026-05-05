# 08 — 常见排错

## 环境问题

### CMD 找不到 multica

**现象**：运行 `multica` 提示 `command not found`

**原因**：multica 未安装，或不在 PATH 中

**解决**：
1. 从官方仓库下载安装包并运行 Windows `setup.exe`：`https://github.com/multica-ai/multica`
2. 检查安装路径（通常在 `C:\Users\<用户名>\AppData\Local\Programs\@multicadesktop\resources\...\bin\`）
3. 将该路径添加到 Windows 系统环境变量 PATH
4. 重启终端
5. 运行 `multica setup` 完成登录和基础配置

### No server configured

**现象**：`multica daemon status` 返回连接错误

**解决**：运行 `multica setup` 或 `multica login`

### Daemon stopped

**现象**：`multica daemon status` 返回 `stopped`

**解决**：运行 `multica daemon start`
如果启动失败，查看日志：`multica daemon logs`

### Daemon 找不到 claude / codex

**现象**：daemon 日志显示 `no agent CLI found`

**原因**：npm 全局安装的 claude/codex CLI 不在 Windows 系统 PATH 中（只在 Git Bash 的 PATH 中）

**解决**：将 npm 全局 bin 目录添加到 Windows 系统环境变量 PATH：
```
C:\Users\<用户名>\AppData\Roaming\npm
```

---

## Agent 问题

### Issue 创建成功但 agent 不执行

**可能原因**：
1. Daemon 未启动
2. Agent 不在线
3. Agent 已归档（archived）

**排查**：
```bash
multica daemon status
multica agent list --output json
multica issue runs <issue-id> --output json
```

### Agent 名称歧义

**现象**：分配 agent 时匹配到多个

**解决**：使用完整名称（如 `C-Pro-开发主力` 而非 `C-Pro`）
先列出所有 agent 确认名称：`multica agent list --output json`

### Agent 执行失败

**查 run 详情**：
```bash
multica issue runs <issue-id> --output json
multica issue run-messages <task-id> --output json
```

**升级策略**：
- Light agent 失败 → 升级到 Pro agent
- Pro agent 失败两次 → 考虑升级到 GPT/Opus agent
- 不要无限循环派单（最多 2 次升级）

---

## Skill 问题

### Skill 没被加载

**现象**：在聊天中提及某个 skill，但 agent 似乎不知道它的存在

**解决**：使用 `Skill(skill="skill-name")` 显式加载

### Skill 分配给了不合适的 agent

**检查**：参考 `docs/03-how-to-map-skills.zh.md` 重新分配

---

## Windows 特定问题

### PATH 不包含 npm 全局目录

multica daemon 是 Windows 原生进程，不会继承 Git Bash 的 PATH 翻译。

**验证**：在 Windows CMD（非 Git Bash）中运行：
```
where claude
where codex
```
如果找不到，需要手动添加 `%APPDATA%\npm` 到 PATH。

---

## 路径占位符

本文档中所有路径已使用以下占位符替换：

| 占位符 | 说明 |
|--------|------|
| `<用户名>` | Windows 用户名 |
| `<agent名称>` | 你的 agent 名称 |
| `<issue-id>` | issue UUID |
| `<task-id>` | task/run UUID |
