# Week 7 – 使用 Graphite 探索 AI 代码审查

## 作业概述
在本作业中，你将在更高级的代码库上练习智能体驱动开发和 AI 辅助代码审查。你将实现 `week7/docs/TASKS.md` 中的任务，通过测试和手动审查验证你的工作，并将你自己的审查笔记与 AI 生成的代码审查进行比较。

## 开始使用 Graphite
1. 注册 Graphite：https://app.graphite.dev/signup
2. 注册后，你可以申请 30 天免费试用。
3. 30 天后，你可以使用代码 **CS146S** 在他们的教育计划下申请免费的 Graphite。


## 需要做什么
使用你选择的 AI 编码工具（例如 Cursor、Copilot、Claude 等）实现 `week7/docs/TASKS.md` 中的任务。

### 对于每个任务：
   1. 创建一个单独的分支。
   2. 使用你的 AI 工具通过 1-shot 提示实现任务。
   3. 逐行手动审查更改。修复你注意到的问题，并在有帮助的地方添加说明性提交消息。你也可以与同学配对，互相审查对方的代码，而不是审查自己的更改。
   4. 为任务打开一个 Pull Request (PR)。确保你的 PR 包括：
      - 问题描述和你的方法。
      - 执行的测试摘要（包括命令和结果）以及任何添加/更新的测试。
      - 值得注意的权衡、限制或后续工作。
   5. 使用 Graphite Diamond 在 PR 上生成 AI 辅助代码审查。
   6. 在 `writeup.md` 中记录你的 PR 结果。


## 提交内容
在你的 `writeup.md` 中，我们需要以下内容：

- 四个 PR，每个已完成的任务一个，每个包括：
  - 清晰的 PR 描述
  - 相关提交/问题的链接。
  - PR 上可见的 Graphite Diamond AI 审查评论

- 简要反思，涉及以下内容：
  - 你在手动审查中通常做出的评论类型（例如，正确性、性能、安全性、命名、测试缺口、API 形状、UX、文档）。
  - 对每个 PR，**你的**评论与 **Graphite** 的 AI 生成评论的比较。
  - AI 审查何时比你的更好/更差（引用具体示例）
  - 你未来信任 AI 审查的舒适程度，以及何时依赖它们的启发式方法。

## 评分标准（总分 100 分）
- 每个已完成任务 20 分
  - 每个任务的技术正确性和完整性。
  - 代码质量：可读性、命名、结构、错误处理和测试。
  - 手动审查笔记的深思熟虑和深度
  - Graphite Diamond AI 生成的代码审查
- 简要反思 20 分
  - 你的审查与 Graphite 的 AI 审查之间的深入比较
  - 你对 AI 审查的个人舒适程度描述


## 提交说明
1. 确保你已将所有更改推送到远程仓库以供评分。
2. 确保你已将 brentju 和 febielin 都添加为作业仓库的协作者。
2. 通过 Gradescope 提交。

---

# Week 7 – Exploring AI Code Review Using Graphite

## Assignment Overview
In this assignment, you will practice agent-driven development and AI-assisted code review on a more advanced codebase. You will implement the tasks in `week7/docs/TASKS.md`, validate your work with tests and manual review, and compare your own review notes with AI-generated code reviews.

## Get Started with Graphite
1. Sign up for Graphite: https://app.graphite.dev/signup
2. Upon sign up, you can claim your 30-day free trial.
3. After the 30 days, you can use code **CS146S** to claim free Graphite under their education program. 


## What to do
Implement the tasks from `week7/docs/TASKS.md` using an AI coding tool of your choice (e.g. Cursor, Copilot, Claude, etc.).

### For each task:
   1. Create a separate branch.
   2. Implement the task with your AI tool using a 1-shot prompt. 
   3. Manually review the changes line-by-line. Fix issues you notice and add explanatory commit messages where helpful. You may also pair with a classmate to review each other's code instead of reviewing your own changes.
   4. Open a Pull Request (PR) for the task. Ensure your PRs include:
      - Description of the problem and your approach.
      - Summary of testing performed (include commands and results) and any added/updated tests.
      - Notable tradeoffs, limitations, or follow-ups.
   5. Use Graphite Diamond to generate an AI-assisted code review on the PR.
   6. Document the results of your PR in the `writeup.md`.


## Deliverables
In your `writeup.md`, we are looking for the follwoing:

- Four PRs, one per completed task, each with:
  - Clear PR description
  - Links to relevant commits/issues.
  - Graphite Diamond AI review comments visible on the PR

- A brief reflection addressing the following:
  - The types of comments you typically made in your manual reviews (e.g., correctness, performance, security, naming, test gaps, API shape, UX, docs).
  - A comparison of **your** comments vs. **Graphite's** AI-generated comments for each PR.
  - When the AI reviews were better/worse than yours (cite specific examples)
  - Your comfort level trusting AI reviews going forward and any heuristics for when to rely on them.

## Evaluation criteria (100 points total)
- 20 points per completed task
  - Technical correctness and completeness of each task.
  - Code quality: readability, naming, structure, error handling, and tests.
  - Thoughtfulness and depth of manual review notes
  - Graphite Diamond AI generated code review
- 20 points for the brief reflection
  - Insightful comparison between your review and Graphite's AI review
  - Description of your personal comfort level with AI Reviews


## Submission Instructions
1. Make sure you have all changes pushed to your remote repository for grading.
2. Make sure you've added both brentju and febielin as collaborators on your assignment repository.
2. Submit via Gradescope. 
