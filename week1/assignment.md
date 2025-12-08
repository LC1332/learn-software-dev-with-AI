# 第一周 — 提示工程技术

你将通过设计提示词来完成特定任务，以此练习多种提示工程技术。每个任务的说明位于相应源代码文件的顶部。

## 安装
请确保你已经按照顶层 `README.md` 中的说明完成了安装。

## Ollama 安装
我们将使用一个名为 [Ollama](https://ollama.com/) 的工具在本地运行不同的最先进大语言模型。请使用以下方法之一进行安装：

- macOS (Homebrew):
  ```bash
  brew install --cask ollama 
  ollama serve
  ```

- Linux (推荐):
  ```bash
  curl -fsSL https://ollama.com/install.sh | sh
  ```

- Windows:
  从 [ollama.com/download](https://ollama.com/download) 下载并运行安装程序。

验证安装:
```bash
ollama -v
```

在运行测试脚本之前，请确保已拉取以下模型。你只需执行一次（除非之后删除了这些模型）:
```bash
ollama run mistral-nemo:12b
ollama run llama3.1:8b
```

## 技术与源代码文件
- K-shot 提示 — `week1/k_shot_prompting.py`
- 思维链 — `week1/chain_of_thought.py`
- 工具调用 — `week1/tool_calling.py`
- 自洽性提示 — `week1/self_consistency_prompting.py`
- RAG（检索增强生成）— `week1/rag.py`
- 反思 — `week1/reflexion.py`

## 交付物
- 阅读每个文件中的任务描述。
- 设计并运行提示词（查找代码中所有标记为 `TODO` 的地方）。这应该是你唯一需要修改的内容（即不要调整模型）。
- 反复迭代改进结果，直到测试脚本通过。
- 保存每种技术的最终提示词和输出。
- 确保提交完成的每个提示工程技术文件的代码。***请仔细检查所有 `TODO` 都已解决。***

## 评分标准（共 60 分）
- 6 种不同提示工程技术中，每个完成的提示词各得 10 分

---

# Week 1 — Prompting Techniques

You will practice multiple prompting techniques by crafting prompts to complete specific tasks. Each task's instructions are at the top of its corresponding source file.

## Installation
Make sure you have first done the installation described in the top-level `README.md`. 

## Ollama installation
We will be using a tool to run different state-of-the-art LLMs locally on your machine called [Ollama](https://ollama.com/). Use one of the following methods:

- macOS (Homebrew):
  ```bash
  brew install --cask ollama 
  ollama serve
  ```

- Linux (recommended):
  ```bash
  curl -fsSL https://ollama.com/install.sh | sh
  ```

- Windows:
  Download and run the installer from [ollama.com/download](https://ollama.com/download).

Verify installation:
```bash
ollama -v
```

Before running the test scripts, make sure you have the following models pulled. You only need to do this once (unless you remove the models later):
```bash
ollama run mistral-nemo:12b
ollama run llama3.1:8b
```

## Techniques and source files
- K-shot prompting — `week1/k_shot_prompting.py`
- Chain-of-thought — `week1/chain_of_thought.py`
- Tool calling — `week1/tool_calling.py`
- Self-consistency prompting — `week1/self_consistency_prompting.py`
- RAG (Retrieval-Augmented Generation) — `week1/rag.py`
- Reflexion — `week1/reflexion.py`

## Deliverables
- Read the task description in each file.
- Design and run prompts (look for all the places labeled `TODO` in the code). That should be the only thing you have to change (i.e. don't tinker with the model). 
- Iterate to improve results until the test script passes.
- Save your final prompt(s) and output for each technique.
- Make sure to include in your submission the completed code for each prompting technique file. ***Double check that all `TODO`s have been resolved.***

## Evaluation rubric (60 pts total)
- 10 for each completed prompt across the 6 different prompting techniques
