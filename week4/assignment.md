# 第 4 周 — 现实中的自主编码智能体

> ***我们建议在开始之前阅读整个文档。***

本周，你的任务是使用以下 **Claude Code** 功能的任意组合，在此代码库的上下文中构建至少 **2 个自动化**：


- 自定义斜杠命令（提交到 `.claude/commands/*.md`）

- 用于代码库或上下文指导的 `CLAUDE.md` 文件

- Claude SubAgents（协同工作的角色专业化智能体）

- 集成到 Claude Code 中的 MCP 服务器

你的自动化应该有意义地改善开发者工作流程——例如，通过简化测试、文档、重构或数据相关任务。然后，你将使用创建的自动化来扩展 `week4/` 中的入门应用程序。


## 了解 Claude Code
为了更深入地了解 Claude Code 并探索你的自动化选项，请阅读以下两个资源：

1. **Claude Code 最佳实践：** [anthropic.com/engineering/claude-code-best-practices](https://www.anthropic.com/engineering/claude-code-best-practices)

2. **SubAgents 概述：** [docs.anthropic.com/en/docs/claude-code/sub-agents](https://docs.anthropic.com/en/docs/claude-code/sub-agents)

## 探索入门应用程序
最小化全栈入门应用程序，设计为**"开发者命令中心"**。 
- 使用 SQLite（SQLAlchemy）的 FastAPI 后端
- 静态前端（无需 Node 工具链）
- 最小化测试（pytest）
- Pre-commit（black + ruff）
- 用于练习智能体驱动工作流的任务

使用此应用程序作为你的实验场，尝试构建的 Claude 自动化。

### 结构

```
backend/                # FastAPI 应用
frontend/               # 由 FastAPI 提供的静态 UI
data/                   # SQLite 数据库 + 种子数据
docs/                   # 用于智能体驱动工作流的任务
```

### 快速开始

1) 激活你的 conda 环境。

```bash
conda activate cs146s
```

2) （可选）安装 pre-commit 钩子

```bash
pre-commit install
```

3) 运行应用（从 `week4/` 目录）

```bash
make run
```

4) 打开 `http://localhost:8000` 查看前端，打开 `http://localhost:8000/docs` 查看 API 文档。

5) 试用入门应用程序，了解其当前的功能和特性。


### 测试
运行测试（从 `week4/` 目录）
```bash
make test
```

### 格式化/代码检查
```bash
make format
make lint
```

## 第一部分：构建你的自动化（选择 2 个或更多）
现在你已经熟悉了入门应用程序，下一步是构建自动化以增强或扩展它。以下是你可以选择的几个自动化选项。你可以在不同类别之间混合搭配。

在构建自动化时，请在 `writeup.md` 文件中记录你的更改。暂时将*"你如何使用自动化来增强入门应用程序"*部分留空——你将在作业的第二部分回到这里。

### A) Claude 自定义斜杠命令
斜杠命令是用于重复工作流的功能，允许你在 `.claude/commands/` 内的 Markdown 文件中创建可重用的工作流。Claude 通过 `/` 暴露这些命令。


- 示例 1：带覆盖率的测试运行器
  - 名称：`tests.md`
  - 意图：运行 `pytest -q backend/tests --maxfail=1 -x`，如果通过，则运行覆盖率。
  - 输入：可选的标记或路径。
  - 输出：总结失败并建议下一步。
- 示例 2：文档同步
  - 名称：`docs-sync.md`
  - 意图：读取 `/openapi.json`，更新 `docs/API.md`，并列出路由差异。
  - 输出：类似差异的摘要和待办事项。
- 示例 3：重构工具
  - 名称：`refactor-module.md`
  - 意图：重命名模块（例如，`services/extract.py` → `services/parser.py`），更新导入，运行代码检查/测试。
  - 输出：修改文件的清单和验证步骤。

>*提示：保持命令聚焦，使用 `$ARGUMENTS`，并优先使用幂等步骤。考虑将安全工具加入白名单，并使用无头模式以提高可重复性。*

### B) `CLAUDE.md` 指导文件
`CLAUDE.md` 文件在开始对话时自动读取，允许你提供影响 Claude 行为的代码库特定指令、上下文或指导。在代码库根目录（以及可选的 `week4/` 子文件夹）中创建 `CLAUDE.md` 以指导 Claude 的行为。

- 示例 1：代码导航和入口点
  - 包括：如何运行应用、路由所在位置（`backend/app/routers`）、测试所在位置、如何播种数据库。
- 示例 2：样式和安全护栏
  - 包括：工具期望（black/ruff）、可运行的安全命令、要避免的命令，以及代码检查/测试门控。
- 示例 3：工作流片段
  - 包括："当被要求添加端点时，首先编写失败的测试，然后实现，然后运行 pre-commit。"

> *提示：像提示一样迭代 `CLAUDE.md`，保持简洁和可操作，并记录你期望 Claude 使用的自定义工具/脚本。*

### C) SubAgents（角色专业化）

SubAgents 是专业化的 AI 助手，配置为使用自己的系统提示、工具和上下文处理特定任务。设计两个或更多协作的智能体，每个智能体负责单个工作流中的不同步骤。

- 示例 1：TestAgent + CodeAgent
  - 流程：TestAgent 为更改编写/更新测试 → CodeAgent 实现代码以通过测试 → TestAgent 验证。
- 示例 2：DocsAgent + CodeAgent
  - 流程：CodeAgent 添加新的 API 路由 → DocsAgent 更新 `API.md` 和 `TASKS.md`，并检查与 `/openapi.json` 的偏差。
- 示例 3：DBAgent + RefactorAgent
  - 流程：DBAgent 提出模式更改（调整 `data/seed.sql`）→ RefactorAgent 更新模型/模式/路由并修复代码检查问题。

>*提示：使用清单/草稿本，在角色之间重置上下文（`/clear`），并为独立任务并行运行智能体。*

## 第二部分：使用你的自动化 
现在你已经构建了 2+ 个自动化，让我们使用它们！在 `writeup.md` 的*"你如何使用自动化来增强入门应用程序"*部分下，描述你如何利用每个自动化来改进或扩展应用的功能。

例如，如果你实现了自定义斜杠命令 `/generate-test-cases`，请解释你如何使用它与入门应用程序交互和测试。


## 交付物
1) 两个或更多自动化，可能包括：
   - `.claude/commands/*.md` 中的斜杠命令
   - `CLAUDE.md` 文件
   - SubAgent 提示/配置（清晰记录，如有文件/脚本）

2) `week4/` 下的 `writeup.md` 文档，包括：
  - 设计灵感（例如，引用最佳实践和/或子智能体文档）
  - 每个自动化的设计，包括目标、输入/输出、步骤
  - 如何运行它（确切命令）、预期输出，以及回滚/安全注意事项
  - 前后对比（即手动工作流 vs. 自动化工作流）
  - 你如何使用自动化来增强入门应用程序



## 提交说明
1. 确保你已将所有更改推送到远程代码库以供评分。
2. **确保你已将 brentju 和 febielin 添加为你作业代码库的协作者。**
2. 通过 Gradescope 提交。

---

# Week 4 — The Autonomous Coding Agent IRL

> ***We recommend reading this entire document before getting started.***

This week, your task is to build at least **2 automations** within the context of this repository using any combination of the following **Claude Code** features:


- Custom slash commands (checked into  `.claude/commands/*.md`)

- `CLAUDE.md` files for repository or context guidance

- Claude SubAgents (role-specialized agents working together)

- MCP servers integrated into Claude Code

Your automations should meaningfully improve a developer workflow – for example, by streamlining tests, documentation, refactors, or data-related tasks. You will then use the automations you create to expand upon the starter application found in `week4/`.


## Learn about Claude Code
To gain a deeper understanding of Claude Code and explore your automation options, please read through the following two resources:

1. **Claude Code best practices:** [anthropic.com/engineering/claude-code-best-practices](https://www.anthropic.com/engineering/claude-code-best-practices)

2. **SubAgents overview:** [docs.anthropic.com/en/docs/claude-code/sub-agents](https://docs.anthropic.com/en/docs/claude-code/sub-agents)

## Explore the Starter Application
Minimal full‑stack starter application designed to be a **"developer's command center"**. 
- FastAPI backend with SQLite (SQLAlchemy)
- Static frontend (no Node toolchain needed)
- Minimal tests (pytest)
- Pre-commit (black + ruff)
- Tasks to practice agent-driven workflows

Use this application as your playground to experiment with the Claude automations you build.

### Structure

```
backend/                # FastAPI app
frontend/               # Static UI served by FastAPI
data/                   # SQLite DB + seed
docs/                   # TASKS for agent-driven workflows
```

### Quickstart

1) Activate your conda environment.

```bash
conda activate cs146s
```

2) (Optional) Install pre-commit hooks

```bash
pre-commit install
```

3) Run the app (from `week4/` directory)

```bash
make run
```

4) Open `http://localhost:8000` for the frontend and `http://localhost:8000/docs` for the API docs.

5) Play around with the starter application to get a feel for its current features and functionality.


### Testing
Run the tests (from `week4/` directory)
```bash
make test
```

### Formatting/Linting
```bash
make format
make lint
```

## Part I: Build Your Automation (Choose 2 or more)
Now that you're familiar with the starter application, your next step is to build automations to enhance or extend it. Below are several automation options you can choose from. You can mix and match across categories.

As you build your automations, document your changes in the `writeup.md` file. Leave the *"How you used the automation to enhance the starter application"* section empty for now - you will be returning to this in Part II of the assignment.

### A) Claude custom slash commands
Slash commands are a feature for repeated workflows, letting you create reusable workflows in Markdown files inside `.claude/commands/`. Claude exposes these via `/`.


- Example 1: Test runner with coverage
  - Name: `tests.md`
  - Intent: Run `pytest -q backend/tests --maxfail=1 -x` and, if green, run coverage.
  - Inputs: Optional marker or path.
  - Output: Summarize failures and suggest next steps.
- Example 2: Docs sync
  - Name: `docs-sync.md`
  - Intent: Read `/openapi.json`, update `docs/API.md`, and list route deltas.
  - Output: Diff-like summary and TODOs.
- Example 3: Refactor harness
  - Name: `refactor-module.md`
  - Intent: Rename a module (e.g., `services/extract.py` → `services/parser.py`), update imports, run lint/tests.
  - Output: A checklist of modified files and verification steps.

>*Tips: Keep commands focused, use `$ARGUMENTS`, and prefer idempotent steps. Consider allowlisting safe tools and using headless mode for repeatability.*

### B) `CLAUDE.md` guidance files
The `CLAUDE.md` file is automatically read when starting a conversation, allowing you to provide repository-specific instructions, context, or guidance that influence Claude's behavior. Create a `CLAUDE.md` in the repo root (and optionally in `week4/` subfolders) to guide Claude's behavior.

- Example 1: Code navigation and entry points
  - Include: How to run the app, where routers live (`backend/app/routers`), where tests live, how the DB is seeded.
- Example 2: Style and safety guardrails
  - Include: Tooling expectations (black/ruff), safe commands to run, commands to avoid, and lint/test gates.
- Example 3: Workflow snippets
  - Include: "When asked to add an endpoint, first write a failing test, then implement, then run pre-commit."

> *Tips: Iterate on `CLAUDE.md` like a prompt, keep it concise and actionable, and document custom tools/scripts you expect Claude to use.*

### C) SubAgents (role-specialized)

SubAgents are specialized AI assistants configured to handle specific tasks with their own system prompts, tools, and context. Design two or more cooperating agents, each responsible for a distinct step in a single workflow.

- Example 1: TestAgent + CodeAgent
  - Flow: TestAgent writes/updates tests for a change → CodeAgent implements code to pass tests → TestAgent verifies.
- Example 2: DocsAgent + CodeAgent
  - Flow: CodeAgent adds a new API route → DocsAgent updates `API.md` and `TASKS.md` and checks drift against `/openapi.json`.
- Example 3: DBAgent + RefactorAgent
  - Flow: DBAgent proposes a schema change (adjust `data/seed.sql`) → RefactorAgent updates models/schemas/routers and fixes lints.

>*Tips: Use checklists/scratchpads, reset context (`/clear`) between roles, and run agents in parallel for independent tasks.*

## Part II: Put Your Automations to Work 
Now that you've built 2+ automations, let's put them to use! In the `writeup.md` under section *"How you used the automation to enhance the starter application"*, describe how you leveraged each automation to improve or extend the app's functionality.

e.g. If you implemented the custom slash command `/generate-test-cases`, explain how you used it to interact with and test the starter application.


## Deliverables
1) Two or more automations, which may include:
   - Slash commands in `.claude/commands/*.md`
   - `CLAUDE.md` files
   - SubAgent prompts/configuration (documented clearly, files/scripts if any)

2) A write-up `writeup.md` under `week4/` that includes:
  - Design inspiration (e.g. cite the best-practices and/or sub-agents docs)
  - Design of each automation, including goals, inputs/outputs, steps
  - How to run it (exact commands), expected outputs, and rollback/safety notes
  - Before vs. after (i.e. manual workflow vs. automated workflow)
  - How you used the automation to enhance the starter application



## SUBMISSION INSTRUCTIONS
1. Make sure you have all changes pushed to your remote repository for grading.
2. **Make sure you've added both brentju and febielin as collaborators on your assignment repository.**
2. Submit via Gradescope. 



