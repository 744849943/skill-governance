# 使用说明

## 推荐工作流

### 第一步：先调用治理 Skill
在 Desktop Work / Codex 中：

`@Skill Governance 先按规范完成 Entry Gate 和架构判定。本轮不要生成 SKILL.md，不要创建插件，输出 Architecture Decision Record 后 STOP。我的需求是：……`

### 第二步：确认架构
确认以下内容：
- 目标环境
- Skill / Plugin / MCP 选型
- 分发方式
- 本地依赖
- 测试矩阵

### 第三步：再调用 skill-creator / plugin-creator
示例：

`@skill-creator 基于已经批准的 Architecture Decision Record 创建 Skill。不得改变已批准的目标 Surface、Packaging 和 Distribution 结论。`

### 第四步：发布前再次调用治理 Skill
示例：

`@Skill Governance 进入 Release Gate。检查当前实现是否符合已批准架构，并按 Discovery / Registration / Loading / Execution 四层测试目标 Surface。`

只有 PASS 后再发布。

## 为什么这样能真正影响后续 Skill 制作

因为规范不再只是“参考文档”，而是被改造成了两个硬门：

1. Entry Gate：不通过就不允许开始写 Skill。
2. Release Gate：不通过就不允许发布。

并且每次都要求产出同一份 Architecture Decision Record，从而让架构选择成为可检查的事实，而不是临时口头判断。
