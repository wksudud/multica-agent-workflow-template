# Issue Description 模板

> 以下模板用于创建 Multica issue。用 `<占位符>` 替换为实际内容。

---

## 写作任务 Issue 模板

```
标题：[DOC] <简短描述>

目标：<完成什么内容>
背景：<为什么需要、相关上下文>
相关文件：<文件路径>
推荐 agent：<agent完整名称>
推荐 skill：<1-3个skill>
限制条件：
- <字数/格式/引用等限制>
- 不修改任何文件
- 不调用高权限 skill
期望输出：<具体产出物>
验证方式：<如何判断完成>
不要做：
- 不要创建文件
- 不要调用其他 agent
```

---

## 编程任务 Issue 模板

```
标题：[BUG] <简短描述> 或 [FEATURE] <简短描述>

目标：<修复什么 / 实现什么>
背景：<触发条件、影响范围>
相关文件：<文件路径>
推荐 agent：<agent完整名称>
推荐 skill：Coding Lead + karpathy-guidelines
限制条件：
- 不要修改无关文件
- 不要重构相关代码
- 修复后运行测试确保不引入回归
期望输出：<代码改动 + 测试验证>
验证方式：<复现步骤或测试命令>
不要做：
- 不要添加新功能（如只修 bug）
- 不要改变 API 签名
- 不要引入新依赖
```

---

## 中大型任务 ContextPacket 模板

适合：新项目、跨文件修改、公开发布前准备、需要多个 agent 协作的任务。

```
标题：[PROJECT] <项目或任务名称>

Goal / 目标：
<一句话说明要交付什么>

Inputs / 输入：
- <相关文件或公开链接>
- <已有约束或用户偏好>

Expected outputs / 期望产物：
- <文档、代码、Demo、检查报告等>

Constraints / 约束：
- 不上传 token、cookie、API key、私钥
- 不发布本地绝对路径、workspace 名称、真实 issue ID
- 不修改无关文件
- 不自动 push、发布、删除或改账号权限

Acceptance / 验收标准：
- <用户可见结果>
- <测试或检查命令通过>
- <公开内容已脱敏>

Failure boundary / 失败边界：
- 遇到凭据、账号、付款、发布、删除、不可逆操作时停止并询问
- 找不到必要上下文时返回 blocker，不编造

File whitelist / 文件边界：
- <允许修改的路径>

Evidence required / 必须返回的证据：
- changed_paths
- verification commands and results
- assumptions
- blockers
- next action
```

---

## 审查任务 Issue 模板

```
标题：[REVIEW] <PR/分支名>

目标：全面审查代码变更的安全性、正确性和可维护性
背景：<变更目的、涉及模块>
相关文件：<文件列表>
推荐 agent：<Review agent名称>
推荐 skill：Agent Git Oracle + Pre Publish Security + karpathy-guidelines
限制条件：
- 不修改代码，只输出审查意见
- 重点检查安全漏洞和边界条件
期望输出：审查报告（问题列表 + 严重程度 + 建议修复方式）
验证方式：所有发现的问题都有对应的代码位置和修复建议
不要做：
- 不要直接修改代码
- 不要把格式化问题当严重问题
```

---

## Skill 创建 Issue 模板

```
标题：[SKILL] 创建 <skill名称> skill

目标：创建一个新的 skill，用于 <用途>
背景：<为什么需要这个 skill>
相关文件：
- 目标 skills 目录：.claude/skills/
- 参考已有 skill：<路径>
推荐 agent：<agent名称>
限制条件：
- 只创建或修改目标 skill 目录中的文件
- 不要覆盖其他已有 skill
- 必须包含 YAML frontmatter
- 不安装新依赖
期望输出：完整的 SKILL.md 文件
验证方式：skill 可被系统识别，内容准确
不要做：
- 不要删除已有 skill
- 不要安装依赖
- 不要联网下载未知内容
```

---

## GitHub 分享任务 Issue 模板

```
标题：[SHARE] 准备在 GitHub 分享 <项目名>

目标：检查公开内容并准备 GitHub 评论或 issue 文案
背景：<分享目的、目标讨论区>
相关文件：
- 当前项目所有文件
- 项目公开前安全检查要求
推荐 agent：<agent名称>
推荐 skill：Pre Publish Security
限制条件：
- 不自动发帖
- 不自动 push
- 不自动打开浏览器
期望输出：公开内容检查报告 + GitHub 评论或 issue 文案
验证方式：检查全部通过，文案无个人信息
不要做：
- 不要自动发布
- 不要自动推送
```
