# 09 — ContextPacket、证据化验收与发布边界

> 本页是一个公开安全的工作流升级示例：它不包含真实 workspace、agent ID、issue ID、运行日志或本地路径。

---

## 为什么需要这一层？

当任务变大后，失败往往不是因为 agent 不聪明，而是因为上下文不清楚：

- 目标没有写成可验收产物
- 约束只存在聊天记录里
- agent 不知道哪些文件能改、哪些不能改
- 完成时只说"done"，没有测试或证据
- 发布、push、删除、账号权限这类高风险动作没有边界

ContextPacket / TaskContract 的作用，就是把这些内容在派单前压成一个小契约。

---

## 推荐链路

```
Direction → Contract → Delegation → Verification → Artifact
```

| 阶段 | 说明 | 产物 |
| --- | --- | --- |
| Direction | 用户给方向，说明想解决什么问题 | 目标摘要 |
| Contract | 把目标转成约束、验收、边界 | ContextPacket / TaskContract |
| Delegation | 只把必要上下文交给合适 agent | Issue / compact handoff |
| Verification | 用命令、截图、审查、日志摘要证明结果 | Evidence |
| Artifact | 把结果沉淀成可复用文件 | README、模板、报告、Demo |

---

## 最小 ContextPacket 字段

```yaml
goal: <要完成什么>
inputs:
  - <相关文件、链接、用户材料>
expected_outputs:
  - <最终产物>
constraints:
  - <不能做什么>
acceptance:
  - <什么算完成>
failure_boundary:
  - <遇到什么必须停止>
file_whitelist:
  - <允许读取/修改的路径>
evidence_required:
  - changed_paths
  - verification
  - assumptions
  - blockers
  - next_action
```

---

## 选择性公开原则

如果你想把自己的 Multica 工作流开源，不要整包上传运行配置。只公开可以迁移的方法和模板。

适合公开：

- 抽象后的 agent 分工方法
- ContextPacket / issue 模板
- 脱敏后的 release gate 清单
- 失败复盘的方法论
- 用占位符写的示例

不适合公开：

- 真实 workspace ID、agent ID、skill ID、runtime ID
- 真实 issue ID、run ID、comment ID
- 本地绝对路径和用户名
- token、cookie、API key、私钥
- daemon 日志、运行日志、失败记录原文
- 包含个人文档或真实业务数据的 artifacts

---

## Manager Agent 的收口要求

Manager / 调度 agent 不只是"把活派出去"。它还要做收口：

```text
请返回：
- status: DONE / DONE_WITH_CONCERNS / BLOCKED
- changed_paths: 修改了哪些文件
- verification: 运行了哪些检查，结果是什么
- assumptions: 做了哪些假设
- blockers: 还卡在哪里
- handoff_summary: 下一位 agent 或用户需要知道什么
```

如果没有证据，不应该声称任务完成。

---

## 发布边界

以下动作默认需要用户确认：

- git push
- 创建远程仓库
- 发布网站、文章、包、Release
- 删除文件、仓库、issue、runtime 数据
- 修改账号、权限、付款设置
- 上传包含个人信息或内部记录的材料

在公开模板里，这条规则比自动化更重要。

