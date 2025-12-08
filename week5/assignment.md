# 第 5 周 — 使用 Warp 进行智能体开发

使用 `week5/` 中的应用作为你的实验场。本周的作业与之前的作业类似，但强调 Warp 智能体开发环境和多智能体工作流。

## 了解 Warp
- Warp 智能体开发环境: [warp.dev](https://www.warp.dev/)
- [Warp University](https://www.warp.dev/university?slug=university)


## 探索入门应用
最小化的全栈入门应用。
- 使用 SQLite (SQLAlchemy) 的 FastAPI 后端
- 静态前端（无需 Node 工具链）
- 最小化测试 (pytest)
- Pre-commit (black + ruff)
- 用于练习智能体驱动工作流的任务

使用此应用作为实验场，尝试你构建的 Warp 自动化功能。

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

3) 运行应用（从 `week5/` 目录）

```bash
make run
```

4) 打开 `http://localhost:8000` 访问前端，打开 `http://localhost:8000/docs` 访问 API 文档。

5) 尝试使用入门应用，了解其当前的功能和特性。


### 测试
运行测试（从 `week5/` 目录）
```bash
make test
```

### 格式化/代码检查
```bash
make format
make lint
```

## 第一部分：构建你的自动化（选择 2 个或更多）
从 `week5/docs/TASKS.md` 中选择任务来实现。你的实现必须通过以下两种方式利用 Warp（更多详情如下）：

- A) 使用 Warp Drive 功能 — 例如保存的提示、规则或 MCP 服务器。
- B) 在 Warp 中整合多智能体工作流。

将你的更改集中在 `week5/` 内的后端、前端、逻辑或测试上。
对于每个选定的任务，请注明其难度级别。


### A) Warp Drive 保存的提示、规则、MCP 服务器（必需：至少一个）
创建一个或多个可共享的 Warp Drive 提示、规则或针对此仓库定制的 MCP 服务器集成。示例：
- 带覆盖率和不稳定测试重跑的测试运行器
- 文档同步：从 `/openapi.json` 生成/更新 `docs/API.md`，列出路由差异
- 重构工具：重命名模块，更新导入，运行 lint/测试
- 发布助手：提升版本号，运行检查，准备变更日志片段
- 集成 Git MCP 服务器，使 Warp 能够自主与 Git 交互（创建分支、提交、PR 注释等）

>*提示：保持工作流聚焦，传递参数，使其具有幂等性，并尽可能优先使用无头/非交互式步骤。*

### B) Warp 中的多智能体工作流（必需：至少一个）
运行多智能体会话，其中不同 Warp 标签页中的独立智能体并发处理独立任务。
- 在单独的 Warp 标签页中使用并发智能体执行 `TASKS.md` 中的多个自包含任务。挑战：你能同时运行多少个智能体？

>*提示：[git worktree](https://git-scm.com/docs/git-worktree) 在这里可能很有用，可以防止智能体相互干扰。*


## 第二部分：使用你的自动化
现在你已经构建了 2+ 个自动化，让我们使用它们！在 `writeup.md` 的 *"你如何使用自动化（它解决或加速了哪些痛点）"* 部分，描述你如何利用每个自动化来改进某些工作流。

## 约束和范围
严格在 `week5/` 内工作（后端、前端、逻辑、测试）。除非自动化明确要求且你记录了原因，否则避免更改其他周的内容。


## 交付物
1) 两个或更多 Warp 自动化，可能包括：
   - Warp Drive 工作流/规则（分享链接和/或导出的定义）以及任何辅助脚本
   - 用于协调多个智能体的任何补充提示/剧本

2) 在 `week5/` 下的 `writeup.md` 文档，包括：
   - 每个自动化的设计，包括目标、输入/输出、步骤
   - 前后对比（即手动工作流 vs 自动化工作流）
   - 每个已完成任务使用的自主级别（哪些代码权限、原因以及你如何监督）
   - （如适用）多智能体说明：角色、协调策略以及并发性优势/风险/失败
   - 你如何使用自动化（它解决或加速了哪些痛点）



## 提交说明
1. 确保你已将所有更改推送到远程仓库以供评分。
2. **确保你已将 brentju 和 febielin 添加为你作业仓库的协作者。**
2. 通过 Gradescope 提交。

---

# Week 5 — Agentic Development with Warp

Use the app in `week5/` as your playground. This week mirrors the prior assignment but emphasizes the Warp agentic development environment and multi‑agent workflows.

## Learn about Warp
- Warp Agentic Development Environment: [warp.dev](https://www.warp.dev/)
- [Warp University](https://www.warp.dev/university?slug=university)


## Explore the Starter Application
Minimal full‑stack starter application.
- FastAPI backend with SQLite (SQLAlchemy)
- Static frontend (no Node toolchain needed)
- Minimal tests (pytest)
- Pre-commit (black + ruff)
- Tasks to practice agent-driven workflows

Use this application as your playground to experiment with the Warp automations you build.

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

3) Run the app (from `week5/` directory)

```bash
make run
```

4) Open `http://localhost:8000` for the frontend and `http://localhost:8000/docs` for the API docs.

5) Play around with the starter application to get a feel for its current features and functionality.


### Testing
Run the tests (from `week5/` directory)
```bash
make test
```

### Formatting/Linting
```bash
make format
make lint
```

## Part I: Build Your Automation (Choose 2 or more) 
Select tasks from `week5/docs/TASKS.md` to implement. Your implementation must leverage Warp in both of the following ways (more details below):

- A) Use Warp Drive features — such as saved prompts, rules, or MCP servers.
- (B) Incorporate multi-agent workflows within Warp.

Keep your changes focused on backend, frontend, logic, or tests inside `week5/`.
For each selected task, note its difficulty level.


### A) Warp Drive saved prompts, rules, MCP servers (REQUIRED: at least one)
Create one or more shareable Warp Drive prompts, rules, or MCP server integrations tailored to this repo. Examples:
- Test runner with coverage and flaky‑test re‑run
- Docs sync: generate/update `docs/API.md` from `/openapi.json`, list route deltas
- Refactor harness: rename a module, update imports, run lint/tests
- Release helper: bump versions, run checks, prepare a changelog snippet
- Integrate the Git MCP server to have Warp interact with Git autonomously (creating branches, commits, PR notes, etc)

>*Tips: keep workflows focused, pass arguments, make them idempotent, and prefer headless/non‑interactive steps where possible.*

### B) Multi‑agent workflows in Warp (REQUIRED: at least one)
Run a multi‑agent session where separate agents in different Warp tabs handle independent tasks concurrently. 
- Perform multiple self-contained tasks from `TASKS.md` in separate Warp tabs using concurrent agents. Challenge: how many agents can you have working simultaneously?

>*Tips: [git worktree](https://git-scm.com/docs/git-worktree) may be helpful here to keep agents from clobbering over each other.*


## Part II: Put Your Automations to Work 
Now that you've built 2+ automations, let's put them to use! In the `writeup.md` under section *"How you used the automation (what pain point it resolves or accelerates)"*, describe how you leveraged each automation to improve some workflow.

## Constraints and scope
Work strictly in `week5/` (backend, frontend, logic, tests). Avoid changing other weeks unless the automation explicitly requires it and you document why.


## Deliverables
1) Two or more Warp automations, which may include:
   - Warp Drive workflows/rules (share links and/or exported definitions) and any helper scripts
   - Any supplemental prompts/playbooks used to coordinate multiple agents

2) A write‑up `writeup.md` under `week5/` that includes:
   - Design of each automation, including goals, inputs/outputs, steps
   - Before vs. after (i.e. manual workflow vs. automated workflow)
   - Autonomy levels used for each completed task (which code permissions, why, and how you supervised)
   - (if applicable) Multi‑agent notes: roles, coordination strategy, and concurrency wins/risks/failures
   - How you used the automation (what pain point it resolves or accelerates)



## SUBMISSION INSTRUCTIONS
1. Make sure you have all changes pushed to your remote repository for grading.
2. **Make sure you've added both brentju and febielin as collaborators on your assignment repository.**
2. Submit via Gradescope. 

