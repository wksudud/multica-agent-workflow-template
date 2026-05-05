# 08 — 创建 multica-workflow-bootstrapper Skill

> 将此提示词发送给你的 AI agent，创建工作流初始化向导 skill。

---

请帮我创建一个新的 skill：`multica-workflow-bootstrapper`

## Skill 用途

Multica 工作流初始化向导。用户第一次搭建 Multica 多 agent + skill 工作流时使用。

它会根据用户自己的模型、预算、API 配置描述、已安装 skill、任务类型和个人偏好，生成推荐的 agent 配置、命名、指令、skill 分配和后续安装步骤。

## 核心要求

1. YAML frontmatter（name + description，中英双语）
2. 六种工作模式：
   - Mode A：需求访谈（12 个结构化问题）
   - Mode B：本地扫描（读 agent list + skill 目录）
   - Mode C：配置推荐（6 种方案：A-F）
   - Mode D：生成 agent 指令（名称/模型/定位/skill/完整指令）
   - Mode E：模板安装/复制（10 条安全要求）
   - Mode F：低风险测试（7 层测试流程）
3. 安全规则：不要求 API key、不自动下载执行、不删除已有 skill
4. 输出模板：配置理解/agent 结构/skill 分配/辅助 skill 判断
5. 职责边界：和 cn-skill-router、multica-cli-operator 的区别

## 重要定位

- 这个 skill 不负责日常派单（日常用 cn-skill-router + multica-cli-operator）
- 这个 skill 只负责"从零开始规划和初始化"
- 所有方案都是建议，不是强制配置
- 作者案例只是 Example Case Study，不是标准答案

## 安全要求

- 不要安装依赖
- 不要修改系统配置
- 不要覆盖已有 skill
- 不要求用户输入 API key

## 参考

可以基于 `skills/multica-workflow-bootstrapper/SKILL.md` 创建。
