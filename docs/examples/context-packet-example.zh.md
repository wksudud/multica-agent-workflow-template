# ContextPacket 示例

> 这是脱敏示例。请把所有尖括号内容替换成你自己的信息。

```yaml
schema: ContextPacket.v1
task_id: public-template-demo
goal: >
  将一个本地 Multica 工作流整理成公开安全的 GitHub 模板，
  只保留可复用的方法、模板和示例。

inputs:
  - README.md
  - docs/
  - examples/
  - skills/

expected_outputs:
  - 更新后的 README 入口
  - 新增 ContextPacket 示例
  - 新增发布前检查清单

constraints:
  - 不发布真实 workspace、agent、skill、issue、run ID
  - 不发布本地绝对路径、用户名、邮箱、账号信息
  - 不发布 token、cookie、API key、私钥
  - 不复制 daemon 日志、runtime ledger 或原始运行记录
  - 不自动 push 或发布

acceptance:
  - 新增内容可以被其他用户直接复用
  - 所有示例使用占位符或虚构名称
  - 文档中没有私有 ID、密钥或本地路径
  - Markdown 文件可正常阅读

failure_boundary:
  - 遇到凭据、账号、付款、发布、删除时停止
  - 不确定某段内容是否可公开时，改写成抽象方法，不贴原文
  - 找不到上下文时返回 blocker，不编造

file_whitelist:
  - README.md
  - docs/**
  - examples/**
  - skills/**

evidence_required:
  - changed_paths
  - verification commands and results
  - public-safety scan summary
  - remaining risks
  - next action
```

## 返回格式示例

```text
status: DONE_WITH_CONCERNS
changed_paths:
- README.md
- docs/09-context-contract-and-evidence.zh.md
- examples/context-packet-example.zh.md

verification:
- rg scan for secrets/private IDs/local paths: no matches
- git diff reviewed: only docs/examples changed

assumptions:
- This template is for public sharing, not live workspace maintenance.

blockers:
- Push still requires user confirmation.

handoff_summary:
- The public template now includes contract-first delegation and release gate examples.
```

