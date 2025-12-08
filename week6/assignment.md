# 第 6 周 — 使用 Semgrep 扫描并修复漏洞

## 作业概述
使用 **Semgrep** 对 `week6/` 中提供的应用程序运行静态分析。对发现的问题进行分类，并修复至少 3 个安全问题。在你的报告中，解释 Semgrep 发现了哪些问题以及你是如何修复它们的。


## 了解 Semgrep
Semgrep 是一个开源的静态分析工具，用于搜索代码、查找错误，并强制执行安全防护措施和编码标准。

1. 点击[这里](https://github.com/semgrep/semgrep/blob/develop/README.md)了解 Semgrep。

2. 按照上述链接中的安装说明进行操作。你可以选择使用 **Semgrep Appsec Platform** 或 **CLI 工具**。


## 扫描任务

### 你将扫描的内容
- 后端 Python (FastAPI): `week6/backend/`
- 前端 JavaScript: `week6/frontend/`
- 依赖项: `week6/requirements.txt`
- 配置文件/环境变量（用于密钥）: `week6/` 内的文件


### 运行一般安全扫描以及针对密钥和依赖项的专项扫描。

从**作业仓库根目录**运行以下命令，应用包含代码和密钥规则的精选 CI 风格捆绑包：
```bash
semgrep ci --subdir week6
```

## 任务
1. 选择 Semgrep 识别的任意 3 个问题，使用你选择的 AI 编码工具修复它们。

2. 展示精确的编辑并解释缓解措施（例如，参数化 SQL、更安全的 API、更强的加密、清理的 DOM 写入、受限的 CORS、依赖项升级）。

3. 重要提示：确保修复后应用程序仍能运行，测试仍能通过。

## 交付物 
### 1. 简要发现概述 
- 总结 Semgrep 报告的类别（SAST/密钥/SCA）。
- 注明你选择忽略的任何误报或嘈杂规则及其原因。

### 2. 三个修复（修复前 → 修复后）
对于每个已修复的问题：
- 文件和行号
- Semgrep 标记的规则/类别
- 简要风险描述
- 你的更改（简短的代码差异或解释，AI 编码工具使用情况）
- 为什么这能缓解问题


## 提示
- 优先使用最小化、有针对性的更改来解决根本原因。
- 每次修复后重新运行 Semgrep，以确认发现的问题已解决且未引入新问题。
- 对于依赖项，如果使用了供应链扫描，请记录升级版本并链接到安全公告。


## 提交说明
1. 确保你已将所有更改推送到远程仓库以供评分。
2. 确保你已将 brentju 和 febielin 添加为作业仓库的协作者。
2. 通过 Gradescope 提交。

---

# Week 6 — Scan and Fix Vulnerabilities with Semgrep

## Assignment Overview
Run static analysis against the provided app in `week6/` using **Semgrep**. Triage findings and remediate a minimum of 3 security issues. In your write-up, explain what issues Semgrep surfaced and how you fixed them.


## Learn about Semgrep
Semgrep is an open-source, static analysis tool that searches code, finds bugs, and enforces secure guardrails and coding standards.

1. Click [here](https://github.com/semgrep/semgrep/blob/develop/README.md) to learn about Semgrep.

2. Follow the installation instructions in the link above. It is up to you whether you prefer to use the **Semgrep Appsec Platform** or the **CLI tool**.


## Scan tasks

### What you will scan
- Backend Python (FastAPI): `week6/backend/`
- Frontend JavaScript: `week6/frontend/`
- Dependencies: `week6/requirements.txt`
- Config/env (for secrets): files within `week6/`


### Run a general security scan plus focused scans for secrets and dependencies.

From the **assignment repository root**, run the following command to apply a curated CI-style bundle that includes both code and secrets rules:
```bash
semgrep ci --subdir week6
```

## Task
1. Pick any 3 issues identified by Semgrep and fix them using an AI coding tool of your choice.

2. Show precise edits and explain the mitigation (e.g., parameterized SQL, safer APIs, stronger crypto, sanitized DOM writes, restricted CORS, dependency upgrades).

3. Important: Ensure the app still runs and tests still pass after your fixes.

## Deliverables 
### 1. Brief findings overview 
- Summarize the categories Semgrep reported (SAST/Secrets/SCA).
- Note any false positives or noisy rules you chose to ignore and why.

### 2. Three fixes (before → after)
For each fixed issue:
- File and line(s)
- Rule/category Semgrep flagged
- Brief risk description
- Your change (short code diff or explanation, AI coding tool usage)
- Why this mitigates the issue


## Tips
- Prefer minimal, targeted changes that address the root cause.
- Re‑run Semgrep after each fix to confirm the finding is resolved and no new ones were introduced.
- For dependencies, document upgraded versions and link to advisories if you used supply-chain scanning.


## Submission Instructions
1. Make sure you have all changes pushed to your remote repository for grading.
2. Make sure you've added both brentju and febielin as collaborators on your assignment repository.
2. Submit via Gradescope. 