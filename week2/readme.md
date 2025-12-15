# CS146S：现代软件开发者 —— 第2周权威讲义

## 编码智能体架构与MCP协议实战

---

## 1. 课程导论：从辅助驾驶到全自动代理的演进

### 1.1 软件开发的范式转移

在CS146S的第一周课程中，我们探索了大型语言模型（LLM）作为代码生成助手的潜力。然而，正如Mihail Eric教授所强调的，现代软件开发的边界正在被重新定义。我们正处于一个从**"人机协同（Copilot）"**向**"自主代理（Agentic）"**模式转变的关键历史节点。

> 如果说Copilot是能够理解指令的副驾驶，那么Agent则是具备独立规划、执行和纠错能力的飞行员。

本周课程**"编码智能体的解构（The Anatomy of Coding Agents）"**将深入探讨这一变革的技术核心。根据斯坦福大学2025年秋季学期的教学大纲，本周的学习目标不仅仅是使用工具，而是要**构建工具**。我们将解构智能体的认知架构，并掌握连接隔离AI模型与现实世界数据的通用标准——**模型上下文协议（MCP）**。

这不仅是技术的升级，更是开发哲学的重构：

| 传统软件开发 | Agent驱动的开发 |
|-------------|----------------|
| 编码 → 编译 → 运行 | 规划 → 生成 → 执行 → 观察 → 修正 |
| 线性流程 | 循环迭代 |

理解这一机制，对于完成本周的GitHub作业至关重要。

### 1.2 课程核心目标与作业概览

本周的讲义旨在支持学生完成两个核心的工程任务，这两个任务直接映射了现代AI工程师必须掌握的关键技能：

#### 📌 任务一：从零构建编码智能体（Build a Coding Agent From Scratch）

学生将不依赖复杂的第三方框架（如LangChain或CrewAI），而是使用原生Python代码实现一个最小可行性Agent。该Agent必须具备：
- 读取文件
- 浏览目录
- 修改代码的核心能力

并能在一个闭环中自主解决简单的编程任务。

#### 📌 任务二：构建自定义MCP服务器（Build a Custom MCP Server）

学生将学习并应用Anthropic推出的开放标准MCP，解决LLM与外部工具连接的互操作性难题。作业要求使用Python的`fastmcp`库构建一个具备计算或数据获取能力的服务器，并将其集成到Claude Desktop等宿主环境中。

---

## 2. 第一部分：编码智能体的解构 (The Anatomy of Coding Agents)

### 2.1 认知架构：智能体的四大支柱

在深入代码实现之前，我们需要建立对Agent认知架构的深刻理解。一个非Agent的LLM就像是一个**缸中之脑**，它拥有浩瀚的知识，却无法感知当下，也无法改变世界。要让它成为Agent，我们需要赋予其"身体"。

根据Siddharth Bharath等专家的分析框架，高效的编码智能体由**四大支柱**支撑：

#### 2.1.1 大脑（The Brain）

核心是大语言模型本身（如Claude 3.5 Sonnet, GPT-4o）。它负责：
- **推理（Reasoning）**
- **规划（Planning）**
- **代码生成**

在CS146S中，我们特别强调选择具备强逻辑推理能力的模型。大脑的作用不是简单的文本补全，而是作为**决策引擎**。它需要分析当前状态，决定下一步是读取文件、搜索文档还是编写代码。

#### 2.1.2 指令（The Instructions / System Prompt）

这是Agent的**"元认知"设定**。系统提示词（System Prompt）定义了：
- Agent的**角色**（例如："你是一位资深后端工程师"）
- **行为边界**（"只修改被请求的文件"）
- **操作规范**（"在写入文件前必须先读取内容"）

Mihail Eric在课堂上展示的"Secret Sauce"中，特别强调了System Prompt在**防止模型漂移（Drift）**中的关键作用。

#### 2.1.3 工具（The Tools）

这是Agent与数字世界交互的接口。如果LLM是CPU，工具就是I/O设备。

对于Coding Agent，最基础的工具集包括：

| 工具类型 | 功能 | 作用 |
|---------|------|------|
| **感知类工具** | `list_dir`（列出目录）、`read_file`（读取文件） | 赋予Agent视觉 |
| **操作类工具** | `write_file`（写入文件）、`run_cmd`（执行命令） | 赋予Agent手脚 |

在作业中，你需要亲手实现这些工具的Python函数，并将其暴露给LLM。

#### 2.1.4 记忆（Memory / Context）

LLM是**无状态的**。为了让Agent能够处理多步骤任务，我们需要维护一个持续更新的上下文窗口。这包括：

- **短期记忆：** 当前的对话历史、工具调用的结果、报错信息。
- **长期记忆：** 项目的代码库索引、外部文档等（通常通过RAG实现，但在本周作业中主要通过文件系统探索来模拟）。

### 2.2 ReAct模式：推理与行动的循环

本周作业的核心架构基于 **ReAct (Reason + Act) 范式**。这是一种让模型在执行动作之前显式生成推理轨迹的方法。

#### 2.2.1 理论机制

在ReAct循环中，Agent不仅仅输出最终答案，而是进入一个动态的循环过程：

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   观察 ──► 思考 ──► 行动 ──► 执行 ──► 循环              │
│    ▲                                    │               │
│    └────────────────────────────────────┘               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

1. **观察（Observation）：** Agent接收来自环境的输入（如用户指令或工具返回的结果）。
2. **思考（Thought）：** Agent在内心独白中分析当前情况，规划下一步。
   > "用户想要修复bug，但我不知道bug在哪，我需要先查看文件列表。"
3. **行动（Action）：** Agent决定调用特定的工具（如`list_dir`）及其参数。
4. **执行（Execution）：** 宿主环境运行工具代码，捕获输出。
5. **循环：** 工具的输出作为新的观察结果反馈给Agent，触发下一轮思考。

这个循环一直持续，直到Agent决定**任务完成（Finish）**或达到最大迭代次数。

### 2.3 深度解析：现代Agent的"秘密酱汁"

在幻灯片中，Mihail Eric揭示了Claude等顶尖模型背后的工程优化细节，这些细节区分了玩具Demo与生产级Agent：

| 技术点 | 原理解析 | 在作业中的应用 |
|-------|---------|--------------|
| **前置上下文 (Front-load context)** | 不要等待模型请求，而是在对话开始时就预先注入关键的项目元数据（如文件树结构、核心依赖版本）。 | 在System Prompt初始化时，自动运行一次`list_dir`并将结果包含在初始消息中。 |
| **系统级提醒 (System Reminders)** | 随着上下文变长，模型容易"遗忘"指令。通过在每一轮交互中动态插入简短的`<system-reminder>`标签，强化核心规则。 | 在将工具结果返回给模型时，附带一句提示："Remember to verify your changes." |
| **命令前缀提取 (Command Prefix Extraction)** | 为了提高解析稳定性，与其让模型输出自由文本，不如强制其以特定前缀（如`Action:`）开始，解析器只监听该前缀。 | 在代码解析逻辑中，使用正则表达式精准匹配工具调用块。 |
| **子智能体生成 (Spawning Sub-agents)** | 避免单一上下文过载。主Agent负责规划，将具体任务（如"编写测试用例"）分发给拥有独立上下文的子Agent。 | **进阶挑战：** 尝试设计一个主控循环，当遇到复杂任务时调用另一个独立的Agent实例。 |

---

## 3. 作业实战一：从零构建Coding Agent

### 3.1 环境搭建与项目初始化

我们将使用Python作为宿主语言。根据课程推荐的现代开发实践，建议使用 `uv` 进行依赖管理，它比传统的pip/venv更快且更可靠。

#### 步骤 1：创建项目结构

在终端中执行以下命令：

```bash
# 创建作业目录
mkdir cs146s-week2-agent
cd cs146s-week2-agent

# 初始化Python环境 (推荐Python 3.10+)
uv init
uv venv
source .venv/bin/activate  # macOS/Linux
# .venv\Scripts\activate   # Windows

# 安装核心依赖
# anthropic: 调用Claude API
# python-dotenv: 管理环境变量
# rich: 美化终端输出
uv pip install anthropic python-dotenv rich
```

#### 步骤 2：配置API密钥

在项目根目录创建 `.env` 文件，填入你的Anthropic API Key：

```bash
ANTHROPIC_API_KEY=sk-ant-api03-...
```

> ⚠️ **注意：** 切勿将此文件提交到GitHub！请在 `.gitignore` 中添加 `.env`。

### 3.2 核心工具函数的实现 (Implementing Tools)

作业的核心在于实现Agent与其环境交互的"手"。我们需要实现三个基础函数。

#### 3.2.1 read_file：Agent的阅读能力

此函数允许Agent读取指定路径的文件内容。

```python
import os

def read_file(path: str) -> str:
    """
    读取指定路径的文件内容。
    
    Args:
        path (str): 文件的相对路径或绝对路径。
        
    Returns:
        str: 文件内容或错误信息。
    """
    try:
        if not os.path.exists(path):
            return f"Error: File '{path}' does not exist."
        
        with open(path, 'r', encoding='utf-8') as f:
            content = f.read()
        return content
    except Exception as e:
        return f"Error reading file '{path}': {str(e)}"
```

> 💡 **专家注记：** 在生产环境中，你需要在这里添加**沙箱检查（Path Sandboxing）**，防止Agent读取项目目录以外的敏感文件（如 `/etc/passwd` 或 `~/.ssh/id_rsa`）。可以通过检查 `os.path.abspath(path)` 是否以项目根目录开头来实现。但在本次作业中，我们专注于功能实现。

#### 3.2.2 write_file：Agent的写作能力

此函数赋予Agent修改代码的权力。这是最强大但也最危险的工具。

```python
def write_file(path: str, content: str) -> str:
    """
    将内容写入文件。如果文件不存在则创建，存在则覆盖。
    
    Args:
        path (str): 文件路径。
        content (str): 要写入的完整内容。
        
    Returns:
        str: 操作结果消息。
    """
    try:
        # 确保目录存在
        directory = os.path.dirname(path)
        if directory and not os.path.exists(directory):
            os.makedirs(directory)
            
        with open(path, 'w', encoding='utf-8') as f:
            f.write(content)
        return f"Successfully wrote to '{path}'."
    except Exception as e:
        return f"Error writing to file '{path}': {str(e)}"
```

> 💡 **专家注记：** 全量覆盖（Overwrite）对于大文件来说效率极低且容易出错。更高级的Agent（如Aider或Cursor）通常支持 `apply_diff` 或 `search_and_replace` 工具。但在Week 2的基础作业中，全量覆盖是可接受的起点。

#### 3.2.3 list_dir：Agent的空间感知

此函数帮助Agent探索未知的文件结构。

```python
def list_dir(path: str = ".") -> str:
    """
    列出目录下的所有文件和子文件夹。
    
    Args:
        path (str): 目录路径，默认为当前目录。
    """
    try:
        if not os.path.exists(path):
            return f"Error: Directory '{path}' does not exist."
            
        files = os.listdir(path)
        return "\n".join(files)
    except Exception as e:
        return f"Error listing directory '{path}': {str(e)}"
```

### 3.3 系统提示词工程 (System Prompt Engineering)

系统提示词是Agent的"操作系统"。它不仅定义了身份，还必须严格规定通信协议。为了让我们的Python脚本能解析Agent的意图，我们需要强制LLM输出结构化的数据（如JSON）。

以下是一个经过优化的System Prompt示例，结合了"前置上下文"原则：

```python
SYSTEM_PROMPT = """
You are an expert AI software engineer capable of reading, writing, and executing code.
You act as an autonomous agent to solve user requests.

AVAILABLE TOOLS:
1. read_file(path): Read content of a file.
2. write_file(path, content): Create or overwrite a file.
3. list_dir(path): List files in a directory.

RESPONSE FORMAT:
You must STRICTLY respond in the following JSON format for every turn. Do not add explanations outside the JSON.

{
    "thought": "Your reasoning process here. Analyze the current state and decide the next step.",
    "action": {
        "tool": "tool_name",
        "args": {
            "arg_name": "value"
        }
    }
}

OR, if you have completed the task:

{
    "thought": "I have completed the user's request.",
    "answer": "Final summary of what was done."
}

GUIDELINES:
1. EXPLORE FIRST: If you don't know the file structure, use `list_dir` first.
2. READ BEFORE WRITE: Never overwrite a file without reading it first to understand its context, unless creating a new file.
3. ITERATIVE: Make small changes and verify.
"""
```

### 3.4 核心循环实现 (The Main Loop)

现在我们将所有组件组装成一个主循环。这个脚本模拟了Agent的生命周期：**感知 → 思考 → 行动 → 观察**。

```python
import json
from anthropic import Anthropic
from dotenv import load_dotenv

# 加载环境变量
load_dotenv()
client = Anthropic()

# 初始化对话历史
messages = []

def chat_with_agent(user_input):
    # 将用户输入加入历史
    messages.append({"role": "user", "content": user_input})
    
    while True:
        # 1. 调用 LLM
        print("\n🤖 Agent is thinking...")
        response = client.messages.create(
            model="claude-3-5-sonnet-20240620",
            max_tokens=1024,
            system=SYSTEM_PROMPT,
            messages=messages
        )
        
        response_content = response.content[0].text
        
        # 2. 解析 JSON 响应
        try:
            # 这是一个简化的解析，实际情况可能需要处理Markdown代码块包裹
            agent_response = json.loads(response_content)
        except json.JSONDecodeError:
            print("❌ Error: Agent returned invalid JSON.")
            break
            
        print(f"💭 Thought: {agent_response.get('thought')}")
        
        # 3. 检查任务是否完成
        if "answer" in agent_response:
            print(f"✅ Task Completed: {agent_response['answer']}")
            # 将最终回答加入历史，结束本轮对话
            messages.append({"role": "assistant", "content": response_content})
            return agent_response['answer']
            
        # 4. 执行工具调用
        action = agent_response.get("action")
        tool_name = action.get("tool")
        args = action.get("args")
        
        print(f"🛠️ Executing: {tool_name} with {args}")
        
        tool_result = ""
        if tool_name == "read_file":
            tool_result = read_file(args.get("path"))
        elif tool_name == "write_file":
            tool_result = write_file(args.get("path"), args.get("content"))
        elif tool_name == "list_dir":
            tool_result = list_dir(args.get("path", "."))
        else:
            tool_result = f"Error: Unknown tool '{tool_name}'"
            
        print(f"📄 Result: {tool_result[:100]}...") # 只打印前100字符
        
        # 5. 更新上下文（Observation）
        # 将助手的思考和工具调用的结果分别加入历史
        messages.append({"role": "assistant", "content": response_content})
        messages.append({
            "role": "user", 
            "content": f"Tool execution result for '{tool_name}':\n{tool_result}"
        })
        
        # 循环继续，Agent将看到工具结果并进行下一步思考

if __name__ == "__main__":
    # 示例任务：让Agent创建一个Python脚本并列出当前目录
    chat_with_agent("Please create a file named 'hello.py' that prints 'Hello from Agent', and then check if it exists.")
```

#### 3.4.1 代码关键点分析

1. **JSON模式强制：** 我们通过System Prompt强制模型输出JSON。在更复杂的场景中，可以使用Anthropic的 `tool_use` API（Function Calling），它能更稳定地输出结构化数据。但在本作业"From Scratch"的要求下，基于Prompt的JSON生成是理解底层原理的最佳方式。

2. **上下文维护：** 注意 `messages.append` 的顺序。我们必须保留完整的"对话链"：
   ```
   用户提问 → 助手思考/调用 → 系统返回结果 → 助手继续思考
   ```
   任何环节的缺失都会导致Agent"失忆"。

3. **递归与终止：** `while True` 循环允许Agent连续执行多个工具操作，直到它自行决定输出 `"answer"`。这是Agent与普通Chatbot的根本区别。

---

## 4. 第二部分：模型上下文协议 (MCP) 深度解析

### 4.1 MCP：解决AI时代的"巴别塔"问题

在CS146S的第二节课中，课程引入了**Model Context Protocol (MCP)**。要理解MCP，首先要理解它试图解决的痛点：**M × N 连接问题**。

随着AI生态的爆发，我们拥有了：
- **M 个模型**（Claude, GPT-4, Llama 3...）
- **N 个数据源**（Google Drive, Slack, GitHub, PostgreSQL...）

在MCP出现之前，如果要让每个模型都能访问每个数据源，我们需要构建 **M × N** 个专用的连接器（Connectors）。这导致了巨大的开发浪费和生态碎片化。

```
传统方式：M × N 连接
┌─────────┐     ┌─────────┐
│ Claude  │────►│ GitHub  │
│         │────►│ Slack   │
│         │────►│ DB      │
├─────────┤     ├─────────┤
│ GPT-4   │────►│ GitHub  │
│         │────►│ Slack   │
│         │────►│ DB      │
└─────────┘     └─────────┘

MCP方式：M + N 连接
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Claude  │     │   MCP   │     │ GitHub  │
│ GPT-4   │────►│ Protocol│◄────│ Slack   │
│ Llama   │     │         │     │ DB      │
└─────────┘     └─────────┘     └─────────┘
```

MCP提出了一种**通用的标准协议**（类似于计算机领域的USB接口）：
- 工具开发者（如Linear, Slack）只需构建一个符合MCP标准的Server
- AI应用（如Claude Desktop, Cursor）只需实现一个MCP Client
- 所有的AI应用都可以通过统一的方式连接所有的MCP Server

**复杂度瞬间降低为 M + N。**

### 4.2 MCP 架构详解

MCP采用典型的**客户端-服务器架构**，包含三个核心角色：

#### MCP Host (宿主/客户端)
发起会话的一方（如Claude Desktop App, IDE）。它负责管理与LLM的连接，并将用户的Prompt与MCP Server提供的上下文进行融合。

> **关键点：** Host决定了用户体验，它掌控着何时向用户展示工具调用请求。

#### MCP Server (服务器)
提供能力的一方。它是一个轻量级的网关，负责连接本地或远程的数据源。

> **关键点：** Server本身通常不包含LLM。它只是定义了"我能做什么（Tools）"和"我有什么数据（Resources）"。

#### MCP Client (连接器)
Host内部用于与Server通信的协议实现层。

### 4.2.1 三大核心原语 (Primitives)

MCP协议定义了三种标准化的能力交换方式：

| 原语 | 描述 | 典型用例 |
|-----|------|---------|
| **Tools (工具)** | 可执行的函数。Host发送调用请求，Server执行并返回结果。这是Agent具备行动能力的基础。 | `weather.get_forecast(city='San Francisco')`, `db.execute_sql(...)` |
| **Resources (资源)** | 被动的数据读取接口。类似于GET请求，用于为LLM提供上下文数据。 | `file://logs/error.log`, `notion://pages/123` |
| **Prompts (提示)** | Server预定义的Prompt模板，帮助用户更高效地使用该Server的能力。 | "Help me debug this error log" (自动加载日志资源到上下文) |

### 4.2.2 通信协议 (Transport)

MCP支持多种传输层，但在本次作业中，我们主要关注 **Stdio (标准输入输出)**。

**Stdio Transport:** Host通过启动一个子进程来运行Server脚本。两者通过标准输入（stdin）和标准输出（stdout）交换JSON-RPC消息。

**优势：** 安全（数据不出本地）、简单（无需网络配置）、高效。

> ⚠️ **注意：** 这意味着在编写Server代码时，**绝对不能使用 `print()` 打印调试信息**，因为这会破坏JSON-RPC的消息格式。所有日志必须通过 `stderr` 输出。

---

## 5. 作业实战二：构建自定义MCP服务器

### 5.1 任务目标与工具选择

本周作业的第二部分要求你构建一个自定义的MCP Server。虽然你可以使用底层的SDK，但课程强烈推荐使用 **FastMCP**。这是一个基于Python的高层框架，它利用Python的类型提示（Type Hints）自动生成MCP协议所需的Schema，极大地简化了开发流程。

**作业要求：** 你需要构建一个具备实际功能的Server。典型的选题包括：
- **高级计算器：** 支持加减乘除及更复杂的数学运算。
- **天气查询服务：** 调用NWS (National Weather Service) API 获取实时天气。

我们将以**天气查询服务**为例，因为它更贴近真实的Agent应用场景——连接外部API。

### 5.2 代码实现：Weather MCP Server

我们将构建一个能够查询美国国家气象局（NWS）数据的MCP Server。

#### 步骤 1：安装依赖

```bash
uv pip install "mcp[cli]" httpx
```

- `mcp[cli]`: 包含核心SDK和FastMCP。
- `httpx`: 一个现代的、支持异步的HTTP客户端，用于调用外部API。

#### 步骤 2：编写 `weather.py`

```python
from typing import Any
import httpx
from mcp.server.fastmcp import FastMCP

# 1. 初始化 Server
# 'weather' 是服务器的名称，将在客户端中显示
mcp = FastMCP("weather")

# 定义常量
NWS_API_BASE = "https://api.weather.gov"
USER_AGENT = "weather-app/1.0"  # NWS API 要求提供 User-Agent

async def make_nws_request(url: str) -> dict[str, Any] | None:
    """辅助函数：发送异步 HTTP 请求并处理错误"""
    headers = {
        "User-Agent": USER_AGENT,
        "Accept": "application/geo+json"
    }
    async with httpx.AsyncClient() as client:
        try:
            response = await client.get(url, headers=headers, timeout=30.0)
            response.raise_for_status()
            return response.json()
        except Exception:
            return None

# 2. 定义工具 (Tool)
# 使用装饰器 @mcp.tool() 注册函数
# FastMCP 会自动解析函数签名和文档字符串生成 JSON Schema

@mcp.tool()
async def get_alerts(state: str) -> str:
    """
    Get weather alerts for a US state.
    
    Args:
        state: Two-letter US state code (e.g. CA, NY).
    """
    url = f"{NWS_API_BASE}/alerts/active/area/{state}"
    data = await make_nws_request(url)

    if not data or "features" not in data:
        return "Unable to fetch alerts or no alerts found."

    if not data["features"]:
        return "No active alerts for this state."

    alerts = []
    for feature in data["features"]:
        props = feature["properties"]
        alert = f"""
        Event: {props.get('event', 'Unknown')}
        Severity: {props.get('severity', 'Unknown')}
        Description: {props.get('description', 'No description')}
        """
        alerts.append(alert)

    return "\n---\n".join(alerts)

@mcp.tool()
async def get_forecast(latitude: float, longitude: float) -> str:
    """
    Get weather forecast for a specific location.
    
    Args:
        latitude: Latitude of the location.
        longitude: Longitude of the location.
    """
    # NWS API 需要先通过经纬度获取 grid point，再获取 forecast
    points_url = f"{NWS_API_BASE}/points/{latitude},{longitude}"
    points_data = await make_nws_request(points_url)

    if not points_data:
        return "Unable to fetch location data."

    forecast_url = points_data["properties"]["forecast"]
    forecast_data = await make_nws_request(forecast_url)

    if not forecast_data:
        return "Unable to fetch forecast."

    periods = forecast_data["properties"]["periods"]
    forecasts = []
    # 只返回前5个时段的预报，避免上下文过长
    for period in periods[:5]:
        forecast = f"{period['name']}: {period['detailedForecast']}"
        forecasts.append(forecast)

    return "\n".join(forecasts)

# 3. 运行服务器
if __name__ == "__main__":
    # 使用 stdio 传输层启动
    mcp.run(transport='stdio')
```

### 5.3 代码深度解构与最佳实践

#### 5.3.1 异步编程 (Async/Await)

注意我们在代码中全面使用了 `async` 和 `await`。

**原因：** MCP Server通常是I/O密集型的（等待网络请求）。使用异步编程可以防止服务器在等待NWS响应时阻塞，这对于保持Agent交互的流畅性至关重要。FastMCP 原生支持异步函数。

#### 5.3.2 类型提示与自文档化 (Type Hints & Docstrings)

请观察 `get_forecast` 函数的定义：

```python
async def get_forecast(latitude: float, longitude: float) -> str:
```

FastMCP 会读取这些类型提示，并生成如下的 MCP Tool Definition：

```json
{
  "name": "get_forecast",
  "inputSchema": {
    "type": "object",
    "properties": {
      "latitude": { "type": "number" },
      "longitude": { "type": "number" }
    },
    "required": ["latitude", "longitude"]
  }
}
```

如果你省略了类型提示，LLM可能无法正确传递参数（例如传递字符串而非浮点数），导致运行时错误。

**文档字符串（Docstring）** 同样重要，它直接成为了LLM的"操作手册"。写得越清晰，模型调用越准确。

#### 5.3.3 错误处理原则

在 `make_nws_request` 中，我们捕获了异常并返回 `None` 或错误消息。

**原则：** MCP Tool 应该尽量避免抛出未处理的异常导致Server崩溃。相反，它应该返回一个人类可读（也是机器可读）的错误字符串（例如 `"Unable to fetch data"`）。这样，Agent接收到错误信息后，有机会进行自我修正（例如重试或询问用户）。

### 5.4 调试与集成 (Debugging & Integration)

构建完成后，如何验证它是否工作？

#### 5.4.1 使用 MCP Inspector 调试

MCP官方提供了一个可视化的调试工具，它是开发者的好朋友。

在终端运行：

```bash
# 启动调试器，指向你的脚本
npx @modelcontextprotocol/inspector uv run weather.py
```

这将在浏览器打开 `http://localhost:5173`。

1. **连接检查：** 你应该能看到 "Connected" 状态。
2. **工具列表：** 在左侧栏，你应该能看到 `get_alerts` 和 `get_forecast`。
3. **模拟调用：** 点击 `get_forecast`，输入 `latitude: 37.7749, longitude: -122.4194` (旧金山坐标)，点击 Run。
4. **查看结果：** 右侧应显示 NWS 返回的文本预报。

如果报错，请检查控制台的 `stderr` 输出。

#### 5.4.2 集成到 Claude Desktop

这是作业的最终验收步骤。

**1. 定位配置文件：**
- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

**2. 编辑配置：** 添加你的Server信息。**务必使用绝对路径**，这是新手最常犯的错误。

```json
{
  "mcpServers": {
    "my-weather-server": {
      "command": "uv",
      "args": [
        "run",
        "/Users/username/projects/cs146s-week2-agent/weather.py" 
      ]
    }
  }
}
```

**3. 重启 Claude：** 完全退出并重新打开 Claude Desktop。

**4. 测试：**
- 在输入框右侧，点击"插头"图标，确认 `my-weather-server` 已加载。
- **对话：** 输入 `"What is the weather forecast for San Francisco?"`
  1. Claude 会分析意图。
  2. Claude 会发送 `call_tool` 请求给你的 Python 脚本。
  3. 你的脚本调用 NWS API 并返回结果。
  4. Claude 将结果转化为自然语言回答你。

---

## 6. 第三部分：进阶工程与最佳实践

### 6.1 安全性：沙箱与权限控制

当我们赋予Agent写文件和执行命令的能力时，安全性成为首要问题。在作业中，我们在本地运行，风险可控。但在生产环境中：

| 措施 | 描述 |
|-----|------|
| **Docker化** | Agent的代码执行环境应被隔离在Docker容器或Firecracker微虚拟机中。 |
| **只读模式** | 对于敏感目录，Agent应只有 `read_file` 权限，没有 `write_file` 权限。 |
| **人工介入 (Human-in-the-loop)** | 对于关键操作（如 `delete_file` 或 `push_to_prod`），MCP协议支持要求用户显式批准（User Approval）。在FastMCP中，可以通过配置实现这一点。 |

### 6.2 上下文管理策略

随着对话深入，Context Window会迅速被填满。

- **截断策略：** 在 `read_file` 实现中，如果文件过大（如超过1000行），应只返回部分内容或摘要，避免撑爆内存和Token预算。
- **RAG集成：** 对于大型项目，不能依赖 `list_dir` 漫游。需要引入向量数据库，让Agent通过语义搜索（Semantic Search）定位相关代码片段。

### 6.3 调试Agent的技巧

当你的Agent陷入死循环或产生幻觉时：

1. **查看思维链：** 打印出 `thought` 字段。Agent之所以做错，通常是因为它"想"错了。
2. **检查System Prompt：** 90%的问题可以通过优化Prompt解决。增加负面约束（Negative Constraints，即"不要做什么"）通常很有效。
3. **清理历史：** 这里的历史不仅是Prompt，也包括Agent生成的错误文件。有时Agent会被自己之前写出的错误代码误导。

---

## 7. 结语与作业提交清单

本周的课程不仅是编写Python脚本，更是一次思维升级。你从调用API的用户，变成了定义API的设计者；从编写代码的工程师，变成了设计"能够编写代码的系统"的架构师。

### 作业提交清单

在提交作业前，请对照以下清单自查：

#### Coding Agent 部分

- [ ] **核心循环：** 是否实现了完整的 Thought → Action → Observation 闭环？
- [ ] **工具健壮性：** `read_file` 读取不存在的文件时，是否返回了清晰的错误信息而不是让程序崩溃？
- [ ] **Prompt设计：** System Prompt 是否成功约束Agent输出合法的JSON格式？
- [ ] **演示：** 能否展示Agent自主完成一个简单任务（如"新建一个计算斐波那契数列的脚本并运行它"）？

#### MCP Server 部分

- [ ] **Server启动：** 脚本是否能通过 `mcp.run()` 无报错启动？
- [ ] **工具定义：** 是否使用了正确的类型提示和文档字符串？
- [ ] **外部集成：** 是否成功在 MCP Inspector 或 Claude Desktop 中调用了你的工具？
- [ ] **异步处理：** 对于网络请求，是否使用了 `async/await`？

---

> 现在，打开你的IDE。这一行行代码，正是通往通用人工智能（AGI）应用层的阶梯。
> 
> Mihail Eric教授说："It's that simple." —— 但真正的力量，隐藏在这些简单的接口与协议连接起的无限可能性之中。

