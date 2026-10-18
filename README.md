# Custom Model Context Protocol (MCP) Client in Python

A lightweight Python-based client built to interact with Model Context Protocol (MCP) servers using the **STDIO transport** mechanism. This project demonstrates how to connect to an MCP server, discover capabilities, invoke tools, read dynamic resources via URIs, and render prompt templates through an interactive CLI interface.

---

## 🎯 Learning Objectives & Key Features

This repository provides a hands-on implementation of the core primitives defined in the Model Context Protocol:

- **🛠️ Tools (Actions):** Discover available functions and execute active operations (e.g., `echo`, `write_file`).
- **📁 Resource Templates (Data):** Read static files and dynamically parameterized resource URIs (e.g., `file://resources/{filename}`).
- **📝 Prompts (LLM Guidance):** Render structured prompt templates dynamically with input parameters.
- **🔌 STDIO Transport & Session Lifecycle:** Establish asynchronous local subprocess channels with structured handshake (`session.initialize()`) and proper cleanup via `AsyncExitStack`.

---

## 🏗️ Architecture Overview

The system operates over Standard Input/Output (STDIO) channels using `JSON-RPC 2.0`:

```text
  ┌──────────────┐                       ┌──────────────┐
  │  MCP Client  │ ────── stdin ────────>│  MCP Server  │
  │ (mcp_client) │                       │ (mcp_server) │
  │              │ <───── stdout ────────│  (FastMCP)   │
  └──────────────┘                       └──────────────┘

Client Subprocess Management: Launches mcp_server.py as a child process using Python's asyncio context.Dynamic Discovery: Automatically inspects exposed server tools, resource template patterns, and prompt templates.Interactive Terminal CLI: Translates user inputs into formal JSON-RPC calls and renders structured outputs.📂 Repository StructurePlaintextcustom-MCP-client-with-python/
├── mcp_client.py       # Async Python client with CLI interface & STDIO session management
├── mcp_server.py       # FastMCP server exposing tools, resources, and prompt templates
├── resources/          # Directory exposed via URI templates (file://resources/{filename})
│   ├── project_info.txt
│   ├── README.md
│   └── notes.txt
├── .gitignore          # Git ignore configuration
└── README.md           # Project documentation
⚙️ Setup & Installation1. PrerequisitesPython: 3.10 or higher2. Clone the RepositoryBashgit clone [https://github.com/aiwithnaveedd/custom-MCP-client-with-python.git](https://github.com/aiwithnaveedd/custom-MCP-client-with-python.git)
cd custom-MCP-client-with-python
3. Set Up Virtual Environment & DependenciesBash# Create virtual environment
python3 -m venv mcp_client_env

# Activate virtual environment
# On Linux/macOS:
source mcp_client_env/bin/activate
# On Windows:
mcp_client_env\Scripts\activate

# Install required packages
pip install mcp==1.16.0 fastmcp==2.12.5
🚀 Running the ApplicationLaunch the custom MCP client by passing the server script path as an argument:Bashpython mcp_client.py mcp_server.py
Interactive CLI CommandsCommandDescriptionExample Input / ParameterstoolsList all tools exposed by the serverN/AcallExecute a tool with JSON argumentsTool: echo | Args: {"text": "Hello MCP!"}resourcesList resource URI template patternsN/AreadRead contents of a resource URIURI: file://resources/project_info.txtpromptsList available prompt templatesN/ApromptRetrieve a rendered prompt templatePrompt: review_file | Args: {"filename": "README.md"}quitGracefully close sessions and exitN/A📄 LicenseThis repository is distributed under the MIT License. See LICENSE for more information.
