# 第 8 周 – 多技术栈 AI 加速 Web 应用构建

## 演示日确认
请访问此[表单](https://forms.gle/J3R3PSRqnFAJxhjG8)了解我们课程演示日的详细信息。


## 作业概述
使用 3 种不同的技术栈构建相同的功能性 Web 应用程序。至少有一个版本必须使用 [`bolt.new`](https://bolt.new/)（一个 AI 应用生成平台）创建。至少有一个版本必须在前端或后端使用非 JavaScript 语言（例如 Django、Ruby on Rails）。

您可以重用前几周的应用（"开发者控制中心"）或创建您选择的新应用，只要它满足[最低功能范围](#最低功能范围)。该应用应该是端到端功能性的（前端 + 后端 + 持久化，如适用），并展示一致的功能集。

## 最低功能范围
- 用户可以创建、读取、更新和删除主要资源（例如，笔记、任务、帖子）。
- 持久化存储（数据库或基于文件的），适用于相应技术栈。
- 基本验证和错误处理。
- 简单但功能性的 UI，展示主要流程。
- 清晰的说明以在本地运行每个版本（如果部署，则包括部署链接）。

## 技术栈要求
构建同一应用的 3 个独立版本，每个版本使用不同的技术栈。示例：
- MERN (MongoDB, Express, React, Node.js)
- MEVN (MongoDB, Express, Vue.js, Node.js)
- Django + React (或 Vue)
- Flask + Vanilla JS (或 React)
- Next.js + Node (或 NestJS)
- Ruby on Rails (全栈)

提醒：至少有一个版本必须在前端或后端包含非 JavaScript 语言（例如 Python/Django、Ruby/Rails）。


至少有一个版本必须使用 AI 应用生成平台 **[`bolt.new`](https://bolt.new/)** 构建，但您也可以自由探索其他应用生成平台（例如 Lovable、Figma Make）来构建其他版本。


## 了解 Bolt
Bolt 是一个 AI 辅助开发平台，可以从自然语言提示生成网站、Web 应用和移动应用。用户可以用纯文本描述他们的想法，Bolt 在几分钟内生成一个功能性原型——从落地页和电子商务网站到 CRM 和移动工具。了解更多[这里](https://support.bolt.new/building/intro-bolt)。

### 领取您的 Bolt 积分：
1. 找到我们通过电子邮件发送给您的唯一 Bolt 促销代码。
2. 访问 [bolt.new](bolt.new) 并创建账户。
3. 在个人设置 > 订阅和令牌中，在"升级到 Pro"块中，点击蓝色的"升级"按钮。
3. 选择"添加促销代码"并将您的唯一促销代码粘贴到此字段中。
4. 您将免费获得 3 个月的 Bolt Pro。需要信用卡来激活试用。**如果您不打算继续订阅，请记住在 3 个月期限结束前取消，以避免自动计费。**


## AI 应用生成器使用技巧
- 像 Bolt 这样的应用生成器最适合现代全栈技术，如果您不指定特定框架，默认会使用这些技术。
- 最好从描述应用概念、实体、路由和 UI 流程的清晰提示开始。
- 在提示中清楚地描述数据模型和关系。
- 迭代优化数据模型、CRUD 端点、身份验证（如果使用）和前端组件的提示。
- 保持每个版本隔离，以避免依赖冲突。
- 导出或同步生成的代码，并将其作为该技术栈的独立项目文件夹提交。
 
## 交付物
1) **三个**项目文件夹（每个版本一个）在 `week8/` 文件夹内，每个包括：
   - 源代码
   - `README.md`，包含先决条件、安装/设置说明、运行和环境配置
   - 关于偏差、已知问题以及生成后任何手动修复的注释
2) 完成的 `writeup.md` 文件：
   - 应用概念
   - 3 个应用描述（每个版本 1 个）

## 评分标准（100 分）
- 应用概念满足最低功能范围（10 分）
- 三个不同的技术栈（10 分）
- 至少在一个版本中使用 Bolt（10 分）
- 至少在一个版本中使用非 JS 语言（10 分）
- 应用的三个版本（每个版本 **20 分**）：
   - 在 `week8/` 文件夹中提供源代码（5 分）
   - README.md：先决条件、安装/设置说明、运行和环境配置（5 分）
   - 应用功能（5 分）
   - 在 `writeup.md` 中详细描述的完整版本描述（5 分）

---

# Week 8 – Multi-Stack AI-Accelerated Web App Build

## Demo Day Confirmation
Please navigate to this [form](https://forms.gle/J3R3PSRqnFAJxhjG8) for details about our class demo day.


## Assignment Overview
Build the same functional web application in 3 distinct technology stacks. At least one version must be created using [`bolt.new`](https://bolt.new/), an AI app generation platform. At least one version must use a non-JavaScript language for either the frontend or backend (e.g., Django, Ruby on Rails).

You may reuse the app from previous weeks (the "developer control center") or create a new app of your choosing, as long as it meets the [minimum functional scope](#minimum-functional-scope). The app should be end-to-end functional (frontend + backend + persistence where applicable) and demonstrate a coherent feature set.

## Minimum Functional Scope 
- User can create, read, update, and delete a primary resource (e.g., notes, tasks, posts).
- Persistent storage (database or file-based) where appropriate for the stack.
- Basic validation and error handling.
- Simple but functional UI that surfaces the main flows.
- Clear instructions to run each version locally (and deploy links if you deploy).

## Stack Requirements
Build 3 separate versions of the same app, each of which use a distinct stack. Examples:
- MERN (MongoDB, Express, React, Node.js)
- MEVN (MongoDB, Express, Vue.js, Node.js)
- Django + React (or Vue)
- Flask + Vanilla JS (or React)
- Next.js + Node (or NestJS)
- Ruby on Rails (full-stack)

Reminder that at least one version must include a non-JavaScript language for either frontend or backend (e.g., Python/Django, Ruby/Rails).


At least one version must be built using the AI app generation platform **[`bolt.new`](https://bolt.new/)**, but feel free to explore other app generation platforms (e.g. Lovable, Figma Make) for the other versions.


## Learn about Bolt
Bolt is an AI-assisted development platform that generates websites, web apps, and mobile apps from natural language prompts. Users can describe their idea in plain text, and Bolt produces a functional prototype—ranging from landing pages and e-commerce sites to CRMs and mobile tools—within minutes. Learn more [here](https://support.bolt.new/building/intro-bolt).

### Claim your Bolt Credits:
1. Locate the unique Bolt promotion code that we've emailed to you.
2. Navigate to [bolt.new](bolt.new) and create an account.
3. In Personal Settings > Subscriptions & Tokens, in the Upgrade to Pro block, click the blue "Upgrade" button.
3. Select "Add promotion code" and paste your unqiue promotion code into this field.
4. You'll receive 3 months of Bolt Pro for free. A credit card is required to activate the trial. **Remember to cancel before the 3-month period ends to avoid automatic billing if you don't plan to continue your subscription.**


## Tips for Usage of AI App Generators
- App generators like Bolt are best-suited for modern full-stack technologies, which you will get by default when using them without specifying specific frameworks.
- Prefer starting from a clean prompt describing your app concept, entities, routes, and UI flows.
- Clearly describe data models and relationships in your prompts.
- Iteratively refine prompts for data models, CRUD endpoints, auth (if used), and frontend components.
- Keep each version isolated to avoid dependency conflicts.
- Export or sync generated code and commit it as a standalone project folder for that stack.
 
## Deliverables
1) **THREE** project folders (one per version) within the `week8/` folder, each including:
   - Source code
   - `README.md` with prerequisites, installation/set-up instructions, run, and env configuration
   - Notes on deviations, known issues, and any manual fixes after generation
2) Completed `writeup.md` file:
   - App Concept
   - 3 App Descriptions (1 per version)

## Grading Rubric (100 points)
- App concept meets minimum functional scope (10 pts)
- Three distinct tech stacks (10 pts)
- Usage of Bolt in at least one version (10 pts)
- Usage of a non-JS language in at least one version (10 pts)
- Three version of the app (20 pts **each**):
   - Source code provided in a folder in `week8/`(5pts)
   - README.md: prerequisites, installation/set-up instructions, run, and env configuration (5 pts)
   - App functionality (5 pts)
   - Complete version description detailed in `writeup.md` (5 pts)

