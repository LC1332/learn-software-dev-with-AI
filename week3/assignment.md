# Week 3 — 构建自定义 MCP 服务器

设计和实现一个包装真实外部 API 的模型上下文协议（MCP）服务器。你可以：
- **本地**运行（STDIO 传输）并与 MCP 客户端（如 Claude Desktop）集成。
- 或**远程**运行（HTTP 传输）并从模型代理或客户端调用。这更难但可以获得额外加分。

如果添加符合 MCP 授权规范的认证（API 密钥或 OAuth2），可获得加分。

## 学习目标
- 理解核心 MCP 能力：工具、资源、提示。
- 实现带有类型化参数和健壮错误处理的工具定义。
- 遵循日志记录和传输最佳实践（STDIO 服务器不使用 stdout）。
- 可选：为 HTTP 传输实现授权流程。

## 要求
1. 选择一个外部 API 并记录你将使用的端点。示例：天气、GitHub issues、Notion 页面、电影/电视数据库、日历、任务管理器、金融/加密货币、旅行、体育统计。
2. 暴露至少两个 MCP 工具
3. 实现基本弹性：
   - 对 HTTP 失败、超时和空结果的优雅错误处理。
   - 遵守 API 速率限制（例如，简单的退避或面向用户的警告）。
4. 打包和文档：
   - 提供清晰的设置说明、环境变量和运行命令。
   - 包含一个示例调用流程（在客户端中要输入/点击什么来触发工具）。
5. 选择一种部署模式：
   - 本地：STDIO 服务器，可从你的机器运行，并可被 Claude Desktop 或像 Cursor 这样的 AI IDE 发现。
   - 远程：可通过网络访问的 HTTP 服务器，可由支持 MCP 的客户端或代理运行时调用。如果已部署且可访问，可获得额外加分。
6. （可选）加分：认证
   - 通过环境变量和客户端配置支持 API 密钥；或
   - 用于 HTTP 传输的 OAuth2 风格承载令牌，验证令牌受众，且绝不将令牌传递给上游 API。

## 交付物
- `week3/` 下的源代码（建议：`week3/server/`，带有清晰的入口点，如 `main.py` 或 `app.py`）。
- `week3/README.md`，包含：
  - 先决条件、环境设置和运行说明（本地和/或远程）。
  - 如何配置 MCP 客户端（本地使用 Claude Desktop 示例）或远程的代理运行时。
  - 工具参考：名称、参数、示例输入/输出和预期行为。

## 评估标准（总分 90 分）
- 功能性（35 分）：实现 2+ 个工具，正确的 API 集成，有意义的输出。
- 可靠性（20 分）：输入验证、错误处理、日志记录、速率限制意识。
- 开发者体验（20 分）：清晰的设置/文档，易于本地运行；合理的文件夹结构。
- 代码质量（15 分）：可读代码、描述性名称、最小复杂度、适用时使用类型提示。
- 额外加分（10 分）：
  - +5 远程 HTTP MCP 服务器，可由代理/客户端（如 OpenAI/Claude SDK）调用。
  - +5 正确实现认证（API 密钥或带受众验证的 OAuth2）。

## 有用的参考资料
- MCP 服务器快速入门：[modelcontextprotocol.io/quickstart/server](https://modelcontextprotocol.io/quickstart/server)。 
*注意：你不能提交这个确切的示例。*
- MCP 授权（HTTP）：[modelcontextprotocol.io/specification/2025-06-18/basic/authorization](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- Cloudflare 上的远程 MCP（代理）：[developers.cloudflare.com/agents/guides/remote-mcp-server/](https://developers.cloudflare.com/agents/guides/remote-mcp-server/)。在部署之前，使用 modelcontextprotocol inspector 工具在本地调试你的服务器。
- https://vercel.com/docs/mcp/deploy-mcp-servers-to-vercel 如果你选择进行远程 MCP 部署，Vercel 是一个不错的选择，提供免费层。

---

# Week 3 — Build a Custom MCP Server

Design and implement a Model Context Protocol (MCP) server that wraps a real external API. You may:
- Run it **locally** (STDIO transport) and integrate with an MCP client (like Claude Desktop).
- Or run it **remotely** (HTTP transport) and call it from a model agent or client. This is harder but earns extra credit.

Bonus points for adding authentication (API keys or OAuth2) aligned with the MCP Authorization spec.

## Learning goals
- Understand core MCP capabilities: tools, resources, prompts.
- Implement tool definitions with typed parameters and robust error handling.
- Follow logging and transport best practices (no stdout for STDIO servers).
- Optionally implement authorization flows for HTTP transports.

## Requirements
1. Choose an external API and document which endpoints you'll use. Examples: weather, GitHub issues, Notion pages, movie/TV databases, calendar, task managers, finance/crypto, travel, sports stats.
2. Expose at least two MCP tools
3. Implement basic resilience:
   - Graceful errors for HTTP failures, timeouts, and empty results.
   - Respect API rate limits (e.g., simple backoff or user-facing warning).
4. Packaging and docs:
   - Provide clear setup instructions, environment variables, and run commands.
   - Include an example invocation flow (what to type/click in the client to trigger the tools).
5. Choose one deployment mode:
   - Local: STDIO server, runnable from your machine and discoverable by Claude Desktop or an AI IDE like Cursor.
   - Remote: HTTP server accessible over the network, callable by an MCP-aware client or an agent runtime. Extra credit if deployed and reachable.
6. (Optional) Bonus: Authentication
   - API key support via environment variable and client configuration; or
   - OAuth2-style bearer tokens for HTTP transport, validating token audience and never passing tokens through to upstream APIs.

## Deliverables
- Source code under `week3/` (suggested: `week3/server/` with a clear entrypoint like `main.py` or `app.py`).
- `week3/README.md` with:
  - Prerequisites, environment setup, and run instructions (local and/or remote).
  - How to configure the MCP client (Claude Desktop example for local) or agent runtime for remote.
  - Tool reference: names, parameters, example inputs/outputs, and expected behaviors.

## Evaluation rubric (90 pts total)
- Functionality (35): Implements 2+ tools, correct API integration, meaningful outputs.
- Reliability (20): Input validation, error handling, logging, rate-limit awareness.
- Developer experience (20): Clear setup/docs, easy to run locally; sensible folder structure.
- Code quality (15): Readable code, descriptive names, minimal complexity, type hints where applicable.
- Extra credit (10):
  - +5 Remote HTTP MCP server, callable by an agent/client such as the OpenAI/Claude SDK.
  - +5 Auth implemented correctly (API key or OAuth2 with audience validation).

## Helpful references
- MCP Server Quickstart: [modelcontextprotocol.io/quickstart/server](https://modelcontextprotocol.io/quickstart/server). 
*NOTE: You may not submit this exact example.*
- MCP Authorization (HTTP): [modelcontextprotocol.io/specification/2025-06-18/basic/authorization](https://modelcontextprotocol.io/specification/2025-06-18/basic/authorization)
- Remote MCP on Cloudflare (Agents): [developers.cloudflare.com/agents/guides/remote-mcp-server/](https://developers.cloudflare.com/agents/guides/remote-mcp-server/). Use the modelcontextprotocol inspector tool to debug your server locally before deploying.
- https://vercel.com/docs/mcp/deploy-mcp-servers-to-vercel If you choose to do a remote MCP deployment, Vercel is a good option with a free tier. 