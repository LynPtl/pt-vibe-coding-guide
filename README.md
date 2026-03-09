# Vibe Coding 实践指南

> 从需求到落地：以规划驱动为核心的 AI 协作开发方法论

---

## 核心理念

Vibe Coding（氛围编程）是由 Andrej Karpathy 于 2025 年提出的新型开发范式——以自然语言驱动、让 LLM 生成大部分代码。本指南整合四篇精华文献，提炼出以「从需求到落地」为主干、「工具与方法论知识」为侧枝的系统化实践框架。

**核心原则：**
- **规划驱动**：拒绝让 AI 自主规划，否则代码库将失控成为巨石文件
- **上下文即一切**：垃圾进，垃圾出；上下文是 vibe coding 的第一性要素
- **模块化优先**：多文件模块，拒绝单体巨文件
- **人机协同**：AI 是执行者，人是决策者与审核者

---

# 主干：从需求到落地

## 阶段一：需求定义

### 1.1 产出 PRD/GDD

无论开发应用还是游戏，第一步都是产出一份清晰的需求文档：

**游戏 → GDD (Game Design Document)**
**应用 → PRD (Product Requirements Document)**

**快速启动提示词：**
```
你是资深软件工程师。我们将一起为一个项目构建 Product Requirements Document。

要点：
- 每次只问一个问题
- 每个问题基于前一个回答展开
- 对每个重要细节深入挖掘

想法：<在此粘贴你的想法>
```

**PRD 应包含的核心章节：**
- 项目概述 (Project Overview)
- 核心需求 (Core Requirements)
- 核心功能 (Core Features)
- 核心组件 (Core Components)
- 用户流程 (App/User Flow)
- 技术栈 (Tech Stack)
- 实施计划 (Implementation Plan)

### 1.2 需求细化技巧

- **不要一次性给出所有需求**：AI 是「yes机器」，需要分步引导
- **给足上下文**：明确编程语言、技术栈、受众群体
- **使用结构化格式**：Markdown 或 AsciiDoc，XML 标签强调重点
- **逆向思维**：从「满足需求」为起点构建，而非从技术出发

---

## 阶段二：技术选型与规划

### 2.1 技术栈选择原则

| 场景 | 推荐技术栈 |
|:---|:---|
| Web 前端 | Next.js + Vercel / React + React Router / TanStack / Remix / FastHTML |
| 后端 | Python + FastAPI / Node.js + Express |
| 小型游戏 | Vanilla JS + Three.js (3D) / PixiJS (2D) |
| 全栈快速原型 | Bolt.new / Lovable → GitHub → 本地 Cursor 继续 |

**选型原则：**
- 优先「最简单但最健壮」的组合
- 前后端分离，避免过早耦合
- 使用 mock/dummy 数据先行，后续接入真实后端

### 2.2 制定实施计划

将 PRD 转化为可执行的分步任务：

```
基于生成的 PRD，创建一个详细的分步实施计划。
将计划拆分为相互依赖的小任务。
确保每个步骤足够小，可一次完成，但又足够大以成功完成项目。
使用软件开发和项目管理的最佳实践，不要有复杂度的剧烈跳跃。
将任务相互关联，创建依赖列表，不应有孤立任务。

要点：
- 使用 markdown 格式
- 每个任务和子任务都是复选框项
- 为每个任务提供足够的上下文
- 每个任务有编号 ID
- 每个任务列出依赖的任务 ID
```

---

## 阶段三：项目初始化

### 3.1 创建与管理目录结构（Memory Bank）

在 Vibe Coding 中，项目目录不仅是存放代码的地方，更是 AI 的“外脑”和上下文记忆库（Memory Bank）。

**标准目录结构参考：**
```text
my-project/
├── docs/
│   ├── specs.md                 # PRD 需求文档
│   └── todo.md                  # 实施任务框架
├── memory-bank/                 # 记忆库（核心真相源）
│   ├── specs.md                 # 业务目标
│   ├── tech-stack.md            # 技术栈与架构原则
│   ├── implementation-plan.md   # 分步实施计划与测试条件
│   ├── progress.md              # 进度记录（当前完成状态）
│   └── architecture.md          # 架构图示与模块职责说明
└── src/                         # 代码目录
```

**如何创建与初始化？**
无需手工新建文件，应利用 AI 一次性生成初始框架：
1. 先建立空项目并初始化版本控制：`mkdir my-project && cd my-project && git init`
2. 使用以下 **Prompt** 让 AI 直接生成并写入结构：

**初始化 Prompt 模板：**
```text
请阅读我的初步项目构思：[在此处粘贴你的初步想法]。
请帮我设计一个标准的 Vibe Coding 项目结构，并在当前工作区直接创建 `memory-bank`（或 `docs`）目录。
请根据我的愿景，自行生成并写入以下文件及内容：
1. `specs.md`（项目需求与目标）
2. `tech-stack.md`（推荐最简且健壮的技术选型记录）
3. `implementation-plan.md`（将开发分解为附带测试验证条件的小任务，格式为 Check-list）
4. `progress.md`（必须保持完全为空，留作后续 AI 记录执行步骤之用）
5. `architecture.md`（必须保持完全为空，留作后续文档化架构说明之用）
请确保 `specs.md`、`tech-stack.md` 和 `implementation-plan.md` 的内容尽量详实，符合最佳工程实践。而 `progress.md` 和 `architecture.md` 仅创建空文件即可。
```

**如何维护与管理工作区内容？**
记忆库的核心价值在于**“实时同步”**。一旦文档与代码脱节，AI 的决策就会崩溃：
- **AI 自动维护纪律**：在项目全局规则（如 `.cursorrules` 或 `CLAUDE.md`）中强制写入此要求：“**每次你完成 `implementation-plan.md` 的任何一个子步骤后，必须主动打开并更新 `progress.md` 记录已做事项；若修改了整体文件结构或数据库表，必须同步更新 `architecture.md`，不得遗漏。**”
- **人类手动纠偏**：如果开发中途代码跑不通且无法推进，人类不仅要执行 `git reset` 撤销错误代码，还必须亲自打开 `progress.md` 和 `architecture.md`，**手动删除这段走入死胡同的错误进展记录**。确保记忆库重回干净无误的状态后，再重新开启新对话。
- **作为所有会话的通用锚点**：每次重启或新建聊天窗口时，你的第一句话永远应当是：“**请先完整读取 `memory-bank/` 里的所有文件，仔细了解当前的进度状态，然后告诉我计划中接下来该做哪一步。**”

### 3.2 配置 Agent 规则

在 Cursor 中创建 `.cursor/rules/` 文件夹，定义项目级规则。

**必须设置的「Always」规则：**
```
# 重要提示：
# 写任何代码前必须完整阅读 memory-bank/@architecture.md（包含完整数据库结构）
# 写任何代码前必须完整阅读 memory-bank/@specs.md
# 每完成一个重大功能或里程碑后，必须更新 memory-bank/@architecture.md
```

**规则创建技巧：**
- 使用 AI 本身生成规则（让 GPT/Claude 读取你的 PRD 和技术栈，自动生成规则）
- 规则强调模块化，禁止单体巨文件
- 包含代码格式、风格、命名规范

---

## 阶段四：迭代开发

### 4.1 核心开发流程与实战 Prompt

在正式让 AI 写代码前，必须执行“消除歧义”步骤，保证 AI 完全理解了所有计划。

**1. 消除歧义 Prompt（开工前）：**
```text
请阅读 `/memory-bank` 文件夹下的所有文档。
请问 `implementation-plan.md` 中的步骤安排对你来说 100% 清晰吗？
在开始写代码前，你还需要我澄清哪些问题，才能让你对接下来要做的事情完全没有任何疑问？
```

**2. 首次执行 Prompt（Step 1）：**
```text
请阅读 `/memory-bank` 下的所有文档，然后执行实施计划的【第 1 步】。
注意：我会负责运行测试并验证。在我向你验证测试通过之前，绝对不要开始第 2 步！
当验证通过后，请你打开 `progress.md` 记录你做了什么（供后续开发者参考），并将新的架构梳理添加到 `architecture.md` 中解释每个文件的作用。
```

**3. 循环迭代 Prompt（Step N，新开聊天窗口）：**
```text
请遍历 `memory-bank` 中的所有文件，并阅读 `progress.md` 了解之前的工作进度。
现在请继续执行实施计划的【第 N 步】。
在我验证测试通过之前，同样不要开始下个步骤。
```

**核心工作流图解：**
```text
┌─────────────────────────────────────────────────────────┐
│  1. 阅读 memory-bank 所有文档                            │
│  2. 消除歧义（向人类提问澄清）                           │
│  3. 使用 Ask/Plan 模式确认当前步骤的方案                 │
│  4. 执行代码生成                                         │
│  5. 人类运行测试验证                                     │
│  6. 测试通过后，AI 更新 progress.md 和 architecture.md   │
│  7. Git 提交代码                                         │
│  8. 记录 Prompt 日志（人类或大模型记录思考过程）         │
│  9. 清理上下文（新建聊天窗口），进入下一步循环           │
└─────────────────────────────────────────────────────────┘
```

### 4.2 分步实施原则

- **小步快跑**：每步完成一个具体任务，不贪多
- **验证驱动**：每步必须有测试，测试通过才继续
- **新聊天窗口**：每次任务开始使用新 chat，让 LLM 保持充足上下文空间
- **先 Ask 后 Act**：先用 Plan 模式确认方案，再执行

### 4.3 Prompt 日志的管理与记录

在 Vibe-Coding 中，代码只是结果，**Prompt 与思考决策才是真正的沉淀**。你必须记录每个核心 Prompt 及其背后的业务考量，便于未来复盘、Debug 和团队移交。

**如何记录？**
你可以自己找个地方手写，但更推荐**将其加入到开发大循环中，让 AI 帮你完成底层文档，你自己补充“思考”。**

**记录 Prompt 模板（加入到每个任务收尾时）：**
```text
执行工作流日志记录协议：
请在项目根目录检查 `history.md`（Prompt 日志）文件是否存在。若不存在请先创建，若存在则在文件末尾追加更新。
请读取我们刚刚完成的这轮代码迭代上下文，自动推断任务名称，并提炼为以下 Markdown 结构写入文件：

```markdown
## {{YYYY-MM-DD}} - 任务：{{自动推断的当前任务简明名称}}

### Prompt
{{高度提炼我向你发送的核心需求或指令意图，剔除冗余沟通词汇}}

### 思考
{{简述你对该阶段任务的架构理解、核心逻辑或关键设计决策}}

### 意外
{{记录执行过程中发生的未覆盖报错、需求变更、踩坑点，或是你产生的被我撤销的过度设计。若完全顺利，则填“无”}}
```

**完整的日志结构示例（history.md）：**

```markdown
## 2026-03-09 - 任务：用户认证模块

### Prompt
实现 JWT 身份验证模块，并确保在中间件拦截无 token 请求。

### 思考
用户需要支持 token 刷新，所以需要设计 refresh token 流程，否则频繁掉线体验极差。

### 意外
AI 自动补充了 Redis 缓存模块，但这超出了目前的需求。沟通后已撤销并删除了 Redis 层代码。
```

---

## 阶段五：调试与修复

### 5.1 错误处理策略

| 错误类型 | 处理方式 |
|:---|:---|
| JavaScript/前端 | 浏览器 F12 控制台 → 复制错误给 AI |
| 后端 | 终端错误信息 → 复制给 AI |
| 视觉问题 | 截图发给 AI |

**调试辅助工具：**
- [BrowserTools](https://browsertools.agentdesk.ai/)：自动捕获浏览器错误和截图

### 5.2 卡壳处理

- **回退 Git**：`git reset` 回到上一个 commit，换 prompt 重试
- **全局视野**：用 [RepoPrompt](https://repoprompt.com/) 或 [uithub](https://uithub.com/) 将整个代码库压缩为单文件，丢给 AI 分析

---

## 阶段六：部署与扩展

### 6.1 部署选项

| 平台 | 适用场景 |
|:---|:---|
| Vercel | Next.js 前端 |
| Render / Fly.io / CloudFlare | 通用后端服务 |
| Railway | 全栈应用 |

### 6.2 功能扩展

- 每新增一个主要功能，创建 `feature-implementation.md`
- 包含：功能描述、分步实现、验收测试
- 继续增量式实现和测试

---

# 侧枝：工具与知识

## 工具篇

### IDE / 代码编辑器

| 工具 | 特点 | 推荐场景 |
|:---|:---|:---|
| **Cursor** | 最多人用，生态完善 | 入门首选 |
| **Claude Code** | 终端原生，深度定制 | 高级用户 |
| **Codex CLI** | OpenAI 出品 | 复杂项目 |
| **Windsurf** | Codeium 出品 | 免费额度 |
| **VSCode + Copilot** | 传统编辑器 + AI | 习惯 VSCode 用户 |

### LLM 模型选择

| 模型 | 适用场景 |
|:---|:---|
| **Claude Opus 4.6** | 复杂逻辑、深度理解 |
| **GPT-4.1** | 通用编码 |
| **GPT-4.1-mini** | 快速简单修改 |
| **gpt-5.3-codex (xhigh)** | 大型项目、复杂逻辑 |
| **Kimi K2 / GLM-4** | 免费/国产模型 |

**模型选择原则：**
- 选最新版本
- 编程任务选支持 tools 的模型
- 参考 [OpenRouter Models](https://openrouter.ai/models?categories=programming&fmt=table) 排行榜

### Web 原型工具

- **Bolt.new**：提示 → 运行 → 编辑 → 部署全栈 Web/移动应用
- **Lovable**：几秒从想法到应用
- **v0 (Vercel)**：构建 Next.js 前端
- **Replit**：描述想法，代理构建

---

## 知识篇

### Prompt 工程要点

1. **分而治之**：不要一次问完，分步骤、分模块
2. **给足细节**：明确语言、框架、受众
3. **强调约束**：「不要做 X」比「做 Y」更重要
4. **强制思考**：「慢慢想，不着急，严格按我说的做」
5. **触发深度思考**（Claude）：`think` < `think hard` < `think harder` < `ultrathink`

### 速率限制应对

- 切换不同模型
- 准备多个 API Key（按量付费，便宜）
- 了解各平台限制（如 [Anthropic](https://console.anthropic.com/settings/limits)）

### 安全实践

- ❌ 不信任 AI 生成的代码 → 始终验证
- ❌ 不在前端硬编码 API Key → 存后端环境变量
- ✅ 查询 API 用 HTTPS
- ✅ 表单输入必做验证与清理
- ❌ 不在 localStorage 存敏感数据
- ✅ 运行安全扫描工具

### MCP (Model Context Protocol)

MCP 是 LLM 与外部工具/资源交互的标准协议：

- **MCP Server**：暴露资源或工具（如数据库、文件系统）
- **MCP Client**：LLM 通过客户端调用服务器
- 当前只有 Anthropic Claude 原生支持 MCP

**常用 MCP 服务器：**
- [BrowserTools](https://browsertools.agentdesk.ai/)：调试辅助
- [Filesystem](https://github.com/modelcontextprotocol/servers)：文件操作
- [GitHub](https://github.com/modelcontextprotocol/servers)：仓库操作

---

## 经验清单

- **目的主导**：一切动作围绕「目的」展开
- **先结构后代码**：想好架构再动手
- **帕累托法则**：关注重要的 20%
- **重复尝试**：AI 生成不好？多试几次
- **模仿优先**：先问 AI 有没有现成仓库可改
- **接口先行**：定义接口，再实现
- **文档即上下文**：不是事后补，是开发中同步写
- **Debug 最小复现**：「预期 vs 实际 + 最小复现步骤」
- **测试 AI 写，断言人审**
- **错误收集成册**：AI 犯的错误整理为经验文档

---

## 推荐资源

### 必读文章

- [The End of Programming as We Know It](https://www.oreilly.com/radar/the-end-of-programming-as-we-know-it) - Tim O'Reilly
- [The 70% Problem: Hard Truths About AI-Assisted Coding](https://addyo.substack.com/p/the-70-problem-hard-truths-about) - Addy Osmani
- [How to Build an Agent](https://ampcode.com/how-to-build-an-agent) - Thorsten Ball
- [Using LLMs for Code](https://simonwillison.net/2025/Mar/11/using-llms-for-code/) - Simon Willison

## 快速启动模板

**给 AI 的第一个提示词：**

```
你是一个专业的 AI 编程助手。我想用 Vibe Coding 的方式开发一个项目。

请先问我：
1. 你想做什么项目？（一句话描述）
2. 你熟悉什么编程语言？（不熟悉也没关系）
3. 你的操作系统是什么？

然后帮我：
1. 推荐最简单的技术栈
2. 生成项目结构
3. 一步步指导我完成开发

要求：每完成一步问我是否成功，再继续下一步。
```

---

**文档基于以下四篇精华文献整理：**
- automata/aicodeguide (Vilson Vieira & Eric S. Raymond)
- vibe-coding-cn (tukuaiai)
- vibe-coding (Nicolas Zullo)
- 各工具官方文档

> 记住：Vibe Coding = 规划驱动 + 上下文固定 + AI 结对执行 = 从想法到可维护代码的流水线。
