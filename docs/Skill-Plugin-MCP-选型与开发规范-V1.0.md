# Skill / Plugin / MCP 选型与开发规范 V1.0

> 适用范围：ChatGPT / Codex 中自研 Skill、Skill-only Plugin、MCP App、Skill + MCP Plugin。
> 核心目标：先决定“在哪里运行、如何分发”，再写 `SKILL.md`，避免“Skill 做出来了，但目标端无法调用”。

---

## 1. 核心原则

### 1.1 先定运行环境，再定实现形态
任何 Skill 项目开始前，必须先明确目标环境：

- Web Chat
- Desktop Chat
- Web Work
- Desktop Work
- Codex
- Mobile（如有）

不得在目标环境未明确前直接进入 `SKILL.md` 编写。

### 1.2 “能发现”不等于“能执行”
对每个目标环境分别验证四层：

1. **发现**：能否通过 `@` / `$` 找到。
2. **注册**：当前账号和当前环境是否拥有有效对象。
3. **加载**：当前会话是否真正获得 Skill 指令或 MCP 工具。
4. **执行**：最终行为是否按 Skill / Tool 规则执行。

只有第 4 层通过，才算“可用”。

### 1.3 单一事实源
同一能力必须只有一个正式源码（Canonical Source）。

允许：
- 本地开发副本
- Git 版本库
- 云端发布产物

不允许：
- 多个同名正式版本长期并存但无人知道哪个是主版本
- 同一内容同时以多个版本号启用
- standalone Skill 与 bundled Skill 长期无治理并存

### 1.4 不为适配界面而过度工程化
如果能力本质只是：
- SOP
- 提示词
- 模板
- 判断规则
- 文档工作流

优先使用 Skill。

只有当能力确实需要：
- 外部系统
- API
- 数据库
- 本地代码/仓库
- 实时数据
- 受控写操作

才引入 MCP。

---

## 2. 架构选型规则

### 2.1 纯工作流
需求只依赖指令、模板、参考资料：

**首选：Standalone Skill**

适用于：
- Work
- Desktop / Codex 等明确支持 standalone Skill 的环境

如果必须稳定覆盖普通 Web/Desktop Chat：
- 优先评估 **Plugin-bundled Skill 的云端发布路径**
- 不应因为 Web Chat 暂不支持 standalone Skill，就直接改造成 MCP

### 2.2 外部工具型能力
需求必须调用外部系统或执行动作：

**首选：MCP App / MCP Plugin**

例如：
- 操作 Git 仓库
- 查询企业系统
- 调数据库
- 发消息
- 调内部 API

### 2.3 工作流 + 工具
既需要稳定流程，又需要外部工具：

**首选：Skill + MCP Plugin**

Skill 负责：
- 调用顺序
- 决策规则
- 输出结构
- 人工确认点
- 失败处理

MCP 负责：
- 数据
- 工具
- 身份认证
- 写操作
- 外部系统连接

---

## 3. 开工前 Entry Gate（必须通过）

每个新 Skill / Plugin 开工前必须回答：

### A. 用户目标
- 用户最终要完成什么？
- 该任务是否高频、可重复？
- 是否需要稳定流程而非一次性提示词？

### B. 目标环境
至少标记 MUST / SHOULD / NOT REQUIRED：

| 环境 | 目标 |
|---|---|
| Web Chat | |
| Desktop Chat | |
| Web Work | |
| Desktop Work | |
| Codex | |
| Mobile | |

### C. 能力依赖
- 是否需要外部数据？
- 是否需要写操作？
- 是否需要本地文件系统？
- 是否需要 Shell？
- 是否需要 localhost？
- 是否需要常驻服务？
- 是否需要认证？

### D. 分发要求
- 仅自己使用
- 团队/工作区使用
- 对外公开
- 是否允许公共审核与发布
- 是否要求 Web Chat 私有使用

### E. 选型结论
必须明确：
- Standalone Skill
- Skill-only Plugin
- MCP App
- Skill + MCP Plugin

未完成上述判定，不进入实现。

---

## 4. Skill Definition 规范

### 4.1 `name`
- 稳定
- 唯一
- kebab-case
- 不随版本变化

### 4.2 `description`
必须写清：
1. 什么时候应该使用
2. 核心任务是什么
3. 什么时候不应该使用

描述用于匹配和触发，不用于承载完整执行逻辑。

### 4.3 `SKILL.md`
必须至少包含：
- Purpose
- Trigger / Scope
- Preconditions
- Inputs
- Workflow
- Decision points
- Human confirmation gates
- Output contract
- Stop conditions
- Error / insufficient evidence handling
- Examples（如必要）

### 4.4 References
- 使用相对路径
- 不依赖本机绝对路径
- 模板和标准尽量放 references
- 核心规则不要只藏在示例里

---

## 5. Packaging 规范

### Standalone Skill
```text
skill-name/
├── SKILL.md
├── references/
├── assets/
└── scripts/        # 仅在确实需要时
```

### Skill-only Plugin
```text
plugin-name/
├── plugin.json
└── skills/
    └── skill-name/
        ├── SKILL.md
        ├── references/
        └── assets/
```

### Skill + MCP Plugin
```text
plugin-name/
├── plugin.json
├── mcp.json
└── skills/
    └── skill-name/
        └── SKILL.md
```

规则：
- 不创建虚假的 MCP 配置
- 不为了“显得完整”增加 hooks
- 不写死个人绝对路径
- 依赖必须可解释、可测试、可移植

---

## 6. Distribution 规范

必须明确区分：

### Local
用于：
- 开发
- 调试
- Codex / Desktop Work
- 本机验证

不得把本地 Marketplace 等同于云端发布。

### Cloud Standalone Skill
用于：
- 支持 standalone Skill 的云端环境
- Work 等可加载该 Skill 的 Surface

不得假设它必然进入普通 Web Chat。

### Workspace Plugin
用于：
- 私有组织分发
- 角色/成员受控
- 不公开发布

### Public Plugin
用于：
- 跨 Web / Desktop / Mobile 的正式分发
- 公共目录
- 审核和版本管理

### Developer-mode MCP App
用于：
- 私有开发态外部工具
- MCP 服务接入
- 需要维护 MCP runtime

不得把它误认为“纯 Skill 的私有云端发布方式”。

---

## 7. Runtime & Portability 规范

除非需求明确要求，否则 Skill 应做到：

- 无本机绝对路径
- 无 localhost
- 无未声明 Shell
- 无隐式本机依赖
- 无必须在线的常驻服务
- references 全部相对路径
- 核心逻辑不依赖聊天历史
- 可在新会话重新加载后独立执行

如果必须依赖本机或服务：
- 明确标注
- 定义断线行为
- 定义错误输出
- 定义恢复方式

---

## 8. Invocation 规范

必须同时设计：

### 显式调用
ChatGPT：
```text
@Skill Name ...
```

Codex：
```text
$skill-name ...
```

### 隐式调用
依赖 `description` 与用户任务匹配。

必须分别测试：
- 显式调用成功
- 自然语言隐式触发成功
- 不应触发时不会误触发

---

## 9. Release Gate（发布前必须通过）

### 9.1 功能测试
至少：
- 5 个正向测试
- 3 个负向测试
- 1 个信息不足测试
- 1 个人工确认测试
- 1 个停止条件测试

### 9.2 Surface 测试
对目标环境逐项记录：

| 环境 | 发现 | 注册 | 加载 | 执行 |
|---|---|---|---|---|
| Web Chat | | | | |
| Desktop Chat | | | | |
| Web Work | | | | |
| Desktop Work | | | | |
| Codex | | | | |

禁止用“@ 能搜到”代替执行验证。

### 9.3 新旧会话测试
安装/更新后至少验证：
- 旧会话
- 新会话
- 新会话优先作为正式验收依据

### 9.4 版本测试
记录：
- Skill commit/hash
- Plugin version
- 云端对象 ID
- 发布时间
- 测试结果

---

## 10. 版本治理

每个能力必须维护：

```text
Canonical Source
      ↓
Git
      ↓
Build / Package
      ↓
Test
      ↓
Release
```

禁止：
- 多个本地 Marketplace 同名版本同时 enabled
- 版本号与内容不一致
- 云端版本更新后未记录对应源码版本

---

## 11. 每个 Skill 必须产出的 Architecture Decision Record

在实现前必须先输出：

```text
# Skill Architecture Decision

## 1. User Goal
## 2. Target Surfaces
## 3. Required Capabilities
## 4. Local / Cloud Dependencies
## 5. Packaging Choice
## 6. Distribution Choice
## 7. Invocation Strategy
## 8. Portability Constraints
## 9. Risks
## 10. Test Matrix
## 11. Final Decision
```

只有用户确认后，才进入实现。

---

## 12. Session Handoff 案例结论

Session Handoff 属于：
- 纯工作流
- 不需要 MCP
- 不需要 Shell
- 不需要 localhost
- 不需要外部实时系统

因此其本质架构仍应保持为 **Skill**。

如果未来需要稳定覆盖普通 Web Chat：
- 优先考虑云端 Plugin-bundled Skill 的正式分发方式
- 若必须私有 Web Chat，目前可用的 Developer-mode 路径本质是 MCP App，需要把 Skill 重构成工具服务，维护成本明显更高
- 不建议仅为了 Web Chat 可用而把 Session Handoff 强行 MCP 化

---

## 13. 一句话判断法

> **先问“这个能力本质上是否需要工具”，再问“我要在哪些界面运行”，最后才决定 Skill、Plugin 还是 MCP。**

如果只是规则和流程：Skill。  
如果要跨普通 Chat 正式分发：Plugin-bundled Skill。  
如果必须访问外部系统：MCP。  
如果既有流程又有工具：Skill + MCP Plugin。
