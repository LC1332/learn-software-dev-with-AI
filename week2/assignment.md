# 第 2 周 – 行动项提取器

本周，我们将扩展一个最小化的 FastAPI + SQLite 应用程序，该应用程序将自由格式的笔记转换为枚举的行动项。

***我们建议在开始之前阅读整个文档。***

提示：要预览此 Markdown 文件
- 在 Mac 上，按 `Command (⌘) + Shift + V`
- 在 Windows/Linux 上，按 `Ctrl + Shift + V`


## 开始使用

### Cursor 设置
按照以下说明设置 Cursor 并打开您的项目：
1. 兑换您的 Cursor Pro 免费一年：https://cursor.com/students
2. 下载 Cursor：https://cursor.com/download
3. 要启用 Cursor 命令行工具，请打开 Cursor，Mac 用户按 `Command (⌘) + Shift+ P`（或非 Mac 用户按 `Ctrl + Shift + P`）打开命令面板。输入：`Shell Command: Install 'cursor' command`。选择它并按 Enter。
4. 打开新的终端窗口，导航到项目根目录，然后运行：`cursor .`

### 当前应用程序
以下是如何启动当前入门应用程序的方法：
1. 激活您的 conda 环境。
```
conda activate cs146s 
```
2. 从项目根目录运行服务器：
```
poetry run uvicorn week2.app.main:app --reload
```
3. 打开 Web 浏览器并导航到 http://127.0.0.1:8000/。
4. 熟悉应用程序的当前状态。确保您可以成功输入笔记并生成提取的行动项清单。

## 练习
对于每个练习，使用 Cursor 帮助您实现对当前行动项提取器应用程序的指定改进。

在完成作业时，使用 `writeup.md` 记录您的进度。请务必包含您使用的提示，以及您或 Cursor 所做的任何更改。我们将根据写出的内容进行评分。还请在代码中添加注释以记录您的更改。

### TODO 1：搭建新功能

分析 `week2/app/services/extract.py` 中现有的 `extract_action_items()` 函数，该函数目前使用预定义的启发式方法提取行动项。

您的任务是实现一个**基于 LLM 的**替代方案 `extract_action_items_llm()`，它利用 Ollama 通过大语言模型执行行动项提取。

一些提示：
- 要生成结构化输出（即字符串的 JSON 数组），请参考此文档：https://ollama.com/blog/structured-outputs
- 要浏览可用的 Ollama 模型，请参考此文档：https://ollama.com/library。请注意，较大的模型将需要更多资源，因此从小模型开始。要拉取并运行模型：`ollama run {MODEL_NAME}`

### TODO 2：添加单元测试

在 `week2/tests/test_extract.py` 中为 `extract_action_items_llm()` 编写单元测试，涵盖多个输入（例如，项目符号列表、关键字前缀行、空输入）。

### TODO 3：重构现有代码以提高清晰度

对后端代码进行重构，特别关注定义良好的 API 合约/模式、数据库层清理、应用程序生命周期/配置、错误处理。

### TODO 4：使用代理模式自动化小任务

1. 将基于 LLM 的提取集成为新端点。更新前端以包含一个"Extract LLM"按钮，单击该按钮时，通过新端点触发提取过程。

2. 公开一个最终端点以检索所有笔记。更新前端以包含一个"List Notes"按钮，单击该按钮时，获取并显示它们。

### TODO 5：从代码库生成 README

***学习目标：***
*学生了解 AI 如何内省代码库并自动生成文档，展示 Cursor 解析代码上下文并将其转换为人类可读形式的能力。*

使用 Cursor 分析当前代码库并生成结构良好的 `README.md` 文件。README 至少应包括：
- 项目的简要概述
- 如何设置和运行项目
- API 端点和功能
- 运行测试套件的说明

## 交付物
根据提供的说明填写 `week2/writeup.md`。确保您的所有更改都在代码库中记录。

## 评分标准（总分 100 分）
- 第 1-5 部分每部分 20 分（生成的代码 10 分，每个提示 10 分）。

---

# Week 2 – Action Item Extractor

This week, we will be expanding upon a minimal FastAPI + SQLite app that converts free‑form notes into enumerated action items.

***We recommend reading this entire document before getting started.***

Tip: To preview this markdown file
- On Mac, press `Command (⌘) + Shift + V`
- On Windows/Linux, press `Ctrl + Shift + V`


## Getting Started

### Cursor Set Up
Follow these instructions to set up Cursor and open your project:
1. Redeem your free year of Cursor Pro: https://cursor.com/students
2. Download Cursor: https://cursor.com/download
3. To enable the Cursor command line tool, open Cursor and press `Command (⌘) + Shift+ P` for Mac users (or `Ctrl + Shift + P` for non-Mac users) to open the Command Palette. Type: `Shell Command: Install 'cursor' command`. Select it and hit Enter.
4. Open a new terminal window, navigate to your project root, and run: `cursor .`

### Current Application
Here's how you can start running the current starter application: 
1. Activate your conda environment.
```
conda activate cs146s 
```
2. From the project root, run the server:
```
poetry run uvicorn week2.app.main:app --reload
```
3. Open a web browser and navigate to http://127.0.0.1:8000/.
4. Familiarize yourself with the current state of the application. Make sure you can successfully input notes and produce the extracted action item checklist. 

## Exercises
For each exercise, use Cursor to help you implement the specified improvements to the current action item extractor application.

As you work through the assignment, use `writeup.md` to document your progress. Be sure to include the prompts you use, as well as any changes made by you or Cursor. We will be grading based on the contents of the write-up. Please also include comments throughout your code to document your changes. 

### TODO 1: Scaffold a New Feature

Analyze the existing `extract_action_items()` function in `week2/app/services/extract.py`, which currently extracts action items using predefined heuristics.

Your task is to implement an **LLM-powered** alternative, `extract_action_items_llm()`, that utilizes Ollama to perform action item extraction via a large language model.

Some  tips:
- To produce structured outputs (i.e. JSON array of strings), refer to this documentation: https://ollama.com/blog/structured-outputs 
- To browse available Ollama models, refer to this documentation: https://ollama.com/library. Note that larger models will be more resource-intensive, so start small. To pull and run a model: `ollama run {MODEL_NAME}`

### TODO 2: Add Unit Tests 

Write unit tests for `extract_action_items_llm()` covering multiple inputs (e.g., bullet lists, keyword-prefixed lines, empty input) in `week2/tests/test_extract.py`.

### TODO 3: Refactor Existing Code for Clarity

Perform a refactor of the code in the backend, focusing in particular on well-defined API contracts/schemas, database layer cleanup, app lifecycle/configuration, error handling. 

### TODO 4: Use Agentic Mode to Automate Small Tasks

1. Integrate the LLM-powered extraction as a new endpoint. Update the frontend to include an "Extract LLM" button that, when clicked, triggers the extraction process via the new endpoint.

2. Expose one final endpoint to retrieve all notes. Update the frontend to include a "List Notes" button that, when clicked, fetches and displays them.

### TODO 5: Generate a README from the Codebase

***Learning Goal:***
*Students learn how AI can introspect a codebase and produce documentation automatically, showcasing Cursor's ability to parse code context and translate it into human‑readable form.*

Use Cursor to analyze the current codebase and generate a well-structured `README.md` file. The README should include, at a minimum:
- A brief overview of the project
- How to set up and run the project
- API endpoints and functionality
- Instructions for running the test suite

## Deliverables
Fill out `week2/writeup.md` according to the instructions provided. Make sure all your changes are documented in your codebase. 

## Evaluation rubric (100 pts total)
- 20 points per part 1-5 (10 for the generated code and 10 for each prompt).
