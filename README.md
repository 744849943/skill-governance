# Skill Governance

`skill-governance` 是一个面向 Skill、Plugin、MCP 与 App 开发流程的架构治理 Meta Skill。它把“应该做成什么、在哪里运行、如何分发、怎样证明可用”放在实现之前，并在发布前重新核验。

## 这是什么

它不是另一个 Skill 生成器，而是两个强制门禁：

- **Entry Gate**：实现前确定目标 Surface、架构形态、分发方式、可移植性和测试矩阵，输出 Skill Architecture Decision Record，并等待用户批准。
- **Release Gate**：实现后按 Discovery、Registration、Runtime Loading、Execution 四层逐项验证，未通过不得宣称发布完成。

## 它解决什么问题

很多 Skill 项目失败，并不是 `SKILL.md` 写错，而是更早的架构决策没有被显式处理：

- 目标 Surface 没有定义；
- Packaging 选择错误；
- Distribution 与目标用户不匹配；
- Local、Cloud、Workspace 与 Public 身份混淆；
- 把“`@` 能看到”误认为“运行时已经加载并执行”；
- 纯规则流程被不必要地 MCP 化；
- 多个同名 standalone Skill、Plugin 或云端对象长期并存，版本失去事实源。

## 核心机制

### Entry Gate

开发前必须明确：

1. User Goal
2. Target Surfaces
3. Required Capabilities
4. Skill / Plugin / MCP / App 选型
5. Packaging 与 Distribution
6. Invocation Strategy
7. Portability Constraints
8. Risks
9. Test Matrix
10. Canonical Source

决策记录使用 [`references/architecture-decision-template.md`](references/architecture-decision-template.md)。完整规范见 [`docs/Skill-Plugin-MCP-选型与开发规范-V1.0.md`](docs/Skill-Plugin-MCP-选型与开发规范-V1.0.md)。

### Release Gate

发布前分别验证：

| 层级 | 要回答的问题 |
|---|---|
| Discovery | 用户能否找到目标 Skill / Plugin / App？ |
| Registration | 当前账号、工作区和目标环境是否拥有有效对象？ |
| Runtime Loading | 新会话是否实际加载了指令或工具？ |
| Execution | 行为是否满足已批准架构和输出契约？ |

此外还应验证新旧会话、版本一致性、正向与负向场景、信息不足、人工确认门和停止条件。

## 什么时候使用

- 新建或大幅修改 Skill；
- 将 standalone Skill 打包成 Plugin；
- 判断是否需要 MCP 或 App；
- 迁移 Local / Cloud / Workspace / Public 分发方式；
- 调整 Web Chat、Desktop Chat、Work、Codex 或 Mobile 兼容性；
- 发布前做架构和运行时验收。

它不应抢占普通写作、编程或资料整理任务；只有当任务涉及创建、修改、打包、迁移或发布 Skill、Plugin、MCP、App 时才应触发。

## 如何安装

将整个仓库目录放入用户级 Skill 目录。当前 Codex 兼容路径示例：

```text
~/.codex/skills/skill-governance/
```

安装后新建 Codex 会话；若列表未刷新，重启 Codex。不要同时保留多个同名正式副本。

## 如何显式调用

Codex：

```text
$skill-governance 先完成 Entry Gate。本轮只输出 Skill Architecture Decision，等待我批准后再实现。
```

ChatGPT Work（若该 Skill 已在目标环境注册）：

```text
@Skill Governance 先判断目标 Surface、Packaging、Distribution 和测试矩阵。
```

## 如何自然语言触发

Skill 默认允许隐式调用。示例：

```text
我想开发一个新的会议纪要 Skill，先帮我做架构判断。
```

自动选择由模型根据 `SKILL.md` 的 `description` 判断，不能以单次测试证明所有请求都会触发。用户级 `AGENTS.md` 可用于把治理流程变成稳定的全局约束。

## 推荐工作流

```text
skill-governance Entry Gate
        ↓
Architecture Decision
        ↓
用户批准
        ↓
skill-creator / plugin-creator
        ↓
实现与行为验证
        ↓
skill-governance Release Gate
        ↓
发布
```

## 与 skill-creator 的关系

| 能力 | 负责的问题 |
|---|---|
| `skill-governance` | 应该做成什么、运行在哪里、如何分发、怎样验收 |
| `skill-creator` | 根据已批准架构创建或更新 Skill 文件与支持资源 |
| `plugin-creator` | 根据已批准架构生成 Plugin 包装和清单 |

治理结论不代替实现工具；实现工具也不能绕过 Entry Gate。

## 目录结构

```text
skill-governance/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── references/
│   ├── architecture-decision-template.md
│   └── skill-development-standard.md
├── docs/
│   ├── architecture-decision-v1.0.md
│   ├── Skill-Plugin-MCP-选型与开发规范-V1.0.md
│   └── validation-report-v1.0.md
├── README.md
├── README_使用说明.md
└── CHANGELOG.md
```

## 版本治理

GitHub 仓库是 canonical source。发布顺序为：Git → package/build → test → release。每次发布应记录 commit、目标 Surface、对象 ID 或版本号，以及四层验收结果。

## License / Contribution

当前仓库尚未选择开源许可证。未经明确许可，不应假设任何特定授权条款。欢迎通过 Issue 或 Pull Request 提交改进建议；贡献内容应保留 Entry Gate、Release Gate 和单一事实源原则。

## English summary

Skill Governance is a meta-skill for architecture decisions before Skill, Plugin, MCP, or App implementation and for evidence-based release validation afterward. It separates discovery, registration, runtime loading, and execution; prevents unnecessary MCP conversion; and requires one canonical source plus an approved Architecture Decision Record. No open-source license has been selected yet.
