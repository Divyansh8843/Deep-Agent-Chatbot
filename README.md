<div align="center">

# Deep Agents with LangChain

### A hands-on course and Streamlit playground for building planning, context-aware, tool-using AI agents.

[![Python](https://img.shields.io/badge/Python-3.13+-3776AB?style=for-the-badge&logo=python&logoColor=white)](#tech-stack)
[![LangChain](https://img.shields.io/badge/LangChain-Agents-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](#tech-stack)
[![LangGraph](https://img.shields.io/badge/LangGraph-Stateful_Agents-111111?style=for-the-badge)](#architecture)
[![Streamlit](https://img.shields.io/badge/Streamlit-Demo-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)](#run-the-streamlit-app)
[![License](https://img.shields.io/badge/License-GPL_v3-blue?style=for-the-badge)](#license)

Learn how to build deep agents that can plan multi-step work, use tools, manage context through files, load reusable skills, delegate to specialized subagents, and preserve memory across conversations.

</div>

---

## Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Course Notebooks](#course-notebooks)
- [Project Structure](#project-structure)
- [Tech Stack](#tech-stack)
- [Quick Start](#quick-start)
- [Environment Variables](#environment-variables)
- [Run the Streamlit App](#run-the-streamlit-app)
- [Backends and Memory](#backends-and-memory)
- [Included Agent Skills](#included-agent-skills)
- [Example Prompts](#example-prompts)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [License](#license)

## Overview

**Deep Agents with LangChain** is a practical learning project for exploring the <code>deepagents</code> library on top of LangChain and LangGraph.

The repository contains four guided Jupyter notebooks and an interactive Streamlit application. Together they demonstrate the core patterns needed for long-running agentic workflows:

- Explicit planning with a todo list
- Context offloading through file-system tools
- Durable instructions through <code>AGENTS.md</code>
- Conversation memory with LangGraph checkpointers
- Swappable storage backends
- Reusable domain skills
- Specialized and structured-output subagents
- Tavily-powered internet research

The Streamlit interface brings these ideas into one configurable chatbot where you can change the model, backend, system prompt, memory, skills, and subagent behavior without editing the application code.

## Key Features

| Capability | What it demonstrates |
| --- | --- |
| Planning | Breaks complex work into trackable steps with <code>write_todos</code>. |
| File tools | Reads, writes, edits, searches, and lists files to keep large context outside the chat history. |
| Context engineering | Loads project guidance from <code>deepagentsdemo/projects/AGENTS.md</code>. |
| Thread memory | Uses a LangGraph <code>MemorySaver</code> checkpointer and unique thread IDs. |
| Multiple backends | Switches between state, local filesystem, and cross-thread store backends. |
| Agent skills | Loads reusable AWS, Python, LangGraph, and report-writing instructions. |
| Subagents | Delegates research to isolated specialist agents. |
| Structured output | Uses a Pydantic schema for research summaries, confidence scores, and sources. |
| Web research | Exposes Tavily search as a custom agent tool. |
| Observable execution | Displays plans, tool calls, subagent tasks, results, and virtual files in Streamlit. |

## Architecture

~~~mermaid
flowchart TD
    U[User] --> UI[Streamlit Chat UI]
    UI --> DA[Deep Agent]

    DA --> P[Planning: write_todos]
    DA --> W[Web Search: Tavily]
    DA --> F[File-System Tools]
    DA --> S[Specialized Subagents]
    DA --> K[Reusable Skills]

    DA <--> C[LangGraph Checkpointer]
    F <--> B{Selected Backend}

    B --> SB[StateBackend]
    B --> FB[FilesystemBackend]
    B --> ST[StoreBackend]

    DA --> UI
    UI --> U
~~~

1. The user submits a request through the Streamlit chat interface.
2. The app builds a deep agent from the selected model and feature settings.
3. The agent plans, searches, delegates, and manages files as needed.
4. A checkpointer preserves conversation state for the active thread.
5. The selected backend controls where agent files and memory are stored.
6. The UI renders the final answer together with expandable execution details.

## Course Notebooks

Work through the notebooks in order:

| Notebook | Topic | Main concepts |
| --- | --- | --- |
| <code>1-basicsdeepagent.ipynb</code> | Deep-agent fundamentals | Agent creation, custom models, prompts, Tavily tools, planning, and file tools |
| <code>2-contextengineering.ipynb</code> | Context engineering | <code>AGENTS.md</code>, memory paths, checkpointers, thread IDs, and skills |
| <code>3-backends.ipynb</code> | Storage backends | <code>StateBackend</code>, <code>FilesystemBackend</code>, and <code>StoreBackend</code> |
| <code>4-subagents.ipynb</code> | Delegation | Custom subagents, context isolation, and Pydantic structured output |

## Project Structure

~~~text
Deep-agents-With-Langchain/
|-- deepagentsdemo/
|   |-- 1-basicsdeepagent.ipynb
|   |-- 2-contextengineering.ipynb
|   |-- 3-backends.ipynb
|   |-- 4-subagents.ipynb
|   |-- notes/
|   |   `-- todo.txt
|   |-- projects/
|   |   `-- AGENTS.md
|   `-- skills/
|       |-- aws/
|       |-- langgraph/
|       |-- python/
|       `-- report-writer/
|-- streamlit_app.py          # Interactive deep-agent playground
|-- main.py                   # Minimal Python entry point
|-- .env.example              # API-key template
|-- requirements.txt          # Full pip dependency list
|-- pyproject.toml            # Project metadata and uv dependencies
|-- uv.lock                   # Locked uv environment
|-- LICENSE                   # GNU GPL v3 license
`-- README.md                # Project documentation
~~~

## Tech Stack

- **Python 3.13+** - application and notebook runtime
- **Deep Agents** - planning, file tools, skills, memory, and subagent orchestration
- **LangChain** - model and tool integrations
- **LangGraph** - state, checkpointers, stores, and agent execution
- **Streamlit** - interactive chatbot interface
- **Tavily** - web-search tool
- **Pydantic** - structured subagent responses
- **OpenAI, Groq, or OpenRouter** - supported model providers
- **Jupyter/IPykernel** - notebook-based course material

## Quick Start

### 1. Open the project

~~~bash
cd Deep-agents-With-Langchain
~~~

### 2. Create a virtual environment

Using Python:

~~~bash
python -m venv .venv
~~~

Activate it on Windows PowerShell:

~~~powershell
.\.venv\Scripts\Activate.ps1
~~~

Activate it on macOS or Linux:

~~~bash
source .venv/bin/activate
~~~

### 3. Install dependencies

~~~bash
python -m pip install --upgrade pip
pip install -r requirements.txt
~~~

Alternatively, with <code>uv</code>:

~~~bash
uv venv
uv pip install -r requirements.txt
~~~

### 4. Create the environment file

Windows PowerShell:

~~~powershell
Copy-Item .env.example .env
~~~

macOS or Linux:

~~~bash
cp .env.example .env
~~~

Add the API keys for the providers you plan to use.

## Environment Variables

~~~dotenv
OPENAI_API_KEY=
GROQ_API_KEY=
TAVILY_API_KEY=
OPENROUTER_API_KEY=
ROUTER_API_KEY=
~~~

| Variable | Purpose | Required |
| --- | --- | --- |
| <code>OPENROUTER_API_KEY</code> | Runs the OpenRouter models shown by default in the Streamlit model selector. | Required for OpenRouter |
| <code>ROUTER_API_KEY</code> | Alternative variable name accepted by the app for OpenRouter. | Optional alias |
| <code>GROQ_API_KEY</code> | Runs the Groq model available in the Streamlit selector. | Required for Groq |
| <code>OPENAI_API_KEY</code> | Runs OpenAI examples used throughout the notebooks. | Required for OpenAI |
| <code>TAVILY_API_KEY</code> | Enables internet search for the main agent and research subagents. | Required for web search |

> Never commit the <code>.env</code> file. It is already excluded by <code>.gitignore</code>.

## Run the Streamlit App

~~~bash
streamlit run streamlit_app.py
~~~

Or:

~~~bash
python -m streamlit run streamlit_app.py
~~~

Streamlit normally opens the application at:

~~~text
http://localhost:8501
~~~

Use the sidebar to:

- Select an OpenRouter or Groq model
- Choose the agent's storage backend
- Enable or disable <code>AGENTS.md</code> context
- Enable or disable skills
- Enable or disable subagents
- Customize the system prompt
- Start a new thread or reset all in-session state

## Backends and Memory

| Backend | Storage location | Lifetime | Best for |
| --- | --- | --- | --- |
| <code>StateBackend</code> | LangGraph state | Current thread with its checkpointer | Temporary working files and isolated conversations |
| <code>FilesystemBackend</code> | Real files under <code>deepagentsdemo/</code> | Persists on disk | Project-aware work and inspecting generated files |
| <code>StoreBackend</code> | Streamlit's in-memory LangGraph store | Cross-thread within the running app session | Demonstrating shared memory between threads |

The current <code>StoreBackend</code> uses <code>InMemoryStore</code>. It can share data across new chat threads while the Streamlit session is running, but it does not survive an application restart.

## Included Agent Skills

| Skill | Purpose |
| --- | --- |
| AWS | AWS architecture, service selection, security, cost guidance, CLI, and boto3 workflows |
| LangGraph | Stateful workflows, graph design, routing, memory, persistence, streaming, and subgraphs |
| Python | Python implementation, debugging, refactoring, typing, testing, and packaging guidance |
| Report Writer | Saves structured Markdown reports for substantive agent responses |

Each skill contains a <code>SKILL.md</code> entry point plus supporting instructions and examples.

## Example Prompts

~~~text
Research the latest approaches to context engineering and cite your sources.
~~~

~~~text
Create a step-by-step plan for a LangGraph customer-support agent with memory.
~~~

~~~text
Compare StateBackend, FilesystemBackend, and StoreBackend for my use case.
~~~

~~~text
Delegate research on serverless AWS architectures and return structured findings.
~~~

~~~text
Write a Python data-processing utility and save a report explaining the design.
~~~

## Troubleshooting

### A model API key is missing

Add the key required by the selected model to <code>.env</code>, then restart Streamlit.

### OpenRouter cannot initialize

Install the full dependency set from <code>requirements.txt</code>. It includes the OpenRouter LangChain integration used by the default model options.

~~~bash
pip install -r requirements.txt
~~~

### Web search fails

Confirm that <code>TAVILY_API_KEY</code> is valid. The chatbot can still answer without search, but research tool calls will fail.

### Notebook imports fail

Make sure the notebook kernel points to the same virtual environment where the project dependencies were installed.

### Files appear inside the project

The <code>FilesystemBackend</code> intentionally gives the agent file access inside the <code>deepagentsdemo/</code> virtual root. Use <code>StateBackend</code> when you want temporary in-state files instead.

## Roadmap

- Add automated tests for agent creation, response parsing, and backend behavior
- Add token streaming and richer execution-status updates
- Add a persistent SQLite or PostgreSQL store
- Add provider-specific model configuration and validation
- Move production secrets to Streamlit secrets or a managed secret store
- Add Docker and cloud deployment examples
- Add evaluation datasets for planning, tool use, and subagent quality

## License

This project is licensed under the **GNU General Public License v3.0**. See [LICENSE](LICENSE) for the full terms.

## Final Note

This repository is designed to make deep-agent architecture tangible. The notebooks explain each capability independently, while the Streamlit app shows how planning, context engineering, memory, skills, tools, and subagents work together in a complete interactive system.

Built for learning, experimentation, and the next generation of long-horizon AI agents.
