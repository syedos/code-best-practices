## What Is Claude Code?

Claude Code is Anthropic's AI-powered coding assistant that runs as:
- A **CLI tool** in the terminal
- A **VS Code extension**
- A **JetBrains plugin**
- A **desktop app** (Mac/Windows)
- A **web app** (claude.ai/code)

It allows developers to interact with Claude directly in their development environment — Claude can read files, write code, run shell commands, search codebases, and perform complex multi-step software engineering tasks autonomously.

## Repository Stats

- **~1,900 TypeScript files**
- **~513,000 lines of code**
- Full client-side implementation — CLI, UI, tools, agent system, and more

## Architecture Overview

### Core Engine

| Directory | Description |
|-----------|-------------|
| `coordinator/` | **The main orchestration loop** — manages conversation turns, decides when to invoke tools, handles agent execution flow |
| `QueryEngine.ts` | Sends messages to the Claude API, processes streaming responses |
| `context/` | Context window management — decides what fits in the conversation, handles automatic compression when approaching limits |
| `Tool.ts` / `tools.ts` | Tool registration, dispatch, and base tool interface |

### Tools (The Core Power of Claude Code)

Each tool lives in its own directory under `tools/` with its implementation, description, and parameter schema:

| Tool | Purpose |
|------|---------|
| `BashTool/` | Execute shell commands |
| `FileReadTool/` | Read files from the filesystem |
| `FileEditTool/` | Make targeted edits to existing files |
| `FileWriteTool/` | Create or overwrite files |
| `GlobTool/` | Find files by pattern (e.g., `**/*.ts`) |
| `GrepTool/` | Search file contents with regex |
| `AgentTool/` | Spawn autonomous sub-agents for complex tasks |
| `WebSearchTool/` | Search the web |
| `WebFetchTool/` | Fetch content from URLs |
| `NotebookEditTool/` | Edit Jupyter notebooks |
| `TodoWriteTool/` | Track task progress |
| `ToolSearchTool/` | Dynamically discover deferred tools |
| `MCPTool/` | Call Model Context Protocol servers |
| `LSPTool/` | Language Server Protocol integration |
| `TaskCreateTool/` | Create background tasks |
| `EnterPlanModeTool/` | Switch to planning mode |
| `SkillTool/` | Execute reusable skill prompts |
| `SendMessageTool/` | Send messages to running sub-agents |

### Terminal UI (Custom Ink-based Renderer)

| Directory | Description |
|-----------|-------------|
| `ink/` | **Custom terminal rendering engine** built on Ink/React with Yoga layout. Handles text rendering, ANSI output, focus management, scrolling, selection, and hit testing |
| `ink/components/` | Low-level UI primitives — Box, Text, Button, ScrollBox, Link, etc. |
| `ink/hooks/` | React hooks for input handling, animation, terminal state |
| `ink/layout/` | Yoga-based flexbox layout engine for the terminal |
| `components/` | Higher-level UI — message display, diff views, prompt input, settings, permissions dialogs, spinners |
| `components/PromptInput/` | The input box where users type |
| `components/messages/` | How assistant/user messages render |
| `components/StructuredDiff/` | Rich diff display for file changes |
| `screens/` | Full-screen views |

### Slash Commands

The `commands/` directory contains **80+ slash commands**, each in its own folder:

- `/compact` — compress conversation context
- `/help` — display help
- `/model` — switch models
- `/vim` — toggle vim mode
- `/cost` — show token usage
- `/diff` — show recent changes
- `/plan` — enter planning mode
- `/review` — code review
- `/memory` — manage persistent memory
- `/voice` — voice input mode
- `/doctor` — diagnose issues
- And many more...

### Services

| Directory | Description |
|-----------|-------------|
| `services/api/` | Anthropic API client and communication |
| `services/mcp/` | MCP (Model Context Protocol) server management |
| `services/lsp/` | Language Server Protocol client for code intelligence |
| `services/compact/` | Conversation compaction/summarization |
| `services/oauth/` | OAuth authentication flow |
| `services/analytics/` | Usage analytics and telemetry |
| `services/extractMemories/` | Automatic memory extraction from conversations |
| `services/plugins/` | Plugin loading and management |
| `services/tips/` | Contextual tips system |

### Permissions & Safety

| Directory | Description |
|-----------|-------------|
| `hooks/toolPermission/` | Permission checking before tool execution |
| `utils/permissions/` | Permission rules and policies |
| `utils/sandbox/` | Sandboxing for command execution |
| `services/policyLimits/` | Rate limiting and policy enforcement |
| `services/remoteManagedSettings/` | Remote settings management for teams |

### Agent System

| Directory | Description |
|-----------|-------------|
| `tools/AgentTool/` | Sub-agent spawning — launches specialized agents for complex tasks |
| `tasks/LocalAgentTask/` | Runs agents locally as sub-processes |
| `tasks/RemoteAgentTask/` | Runs agents on remote infrastructure |
| `tasks/LocalShellTask/` | Shell-based task execution |
| `services/AgentSummary/` | Summarizes agent work |

### Persistence & State

| Directory | Description |
|-----------|-------------|
| `state/` | Application state management |
| `utils/settings/` | User and project settings (settings.json) |
| `memdir/` | Persistent memory directory system |
| `utils/memory/` | Memory read/write utilities |
| `migrations/` | Data format migrations |
| `keybindings/` | Keyboard shortcut configuration |

### Skills & Plugins

| Directory | Description |
|-----------|-------------|
| `skills/` | Skill system — reusable prompt templates (e.g., `/commit`, `/review-pr`) |
| `plugins/` | Plugin architecture for extending functionality |
| `services/plugins/` | Plugin loading, validation, and lifecycle |

### Other Notable Directories

| Directory | Description |
|-----------|-------------|
| `bridge/` | Bridge for desktop/web app communication (session management, JWT auth, polling) |
| `remote/` | Remote execution support |
| `server/` | Server mode for programmatic access |
| `entrypoints/` | App entry points (CLI, SDK) |
| `vim/` | Full vim emulation (motions, operators, text objects) |
| `voice/` | Voice input support |
| `buddy/` | Companion sprite system (fun feature) |
| `cli/` | CLI argument parsing and transport layer |
| `native-ts/` | Native module bindings (color-diff, file-index, yoga-layout) |
| `schemas/` | JSON schemas for configuration |
| `types/` | TypeScript type definitions |

### Key Entry Points

- **`main.tsx`** — Application entry point, bootstraps the Ink-based terminal UI
- **`coordinator/coordinatorMode.ts`** — The core conversation loop
- **`QueryEngine.ts`** — API query engine
- **`tools.ts`** — Tool registry
- **`context.ts`** — Context management
- **`commands.ts`** — Command registry

## Notable Implementation Details

- **Built with TypeScript** and React (via Ink for terminal rendering)
- **Yoga layout engine** for flexbox-style terminal UI
- **Custom vim emulation** with full motion/operator/text-object support
- **MCP (Model Context Protocol)** support for connecting external tool servers
- **LSP integration** for code intelligence features
- **Plugin system** for community extensions
- **Persistent memory** system across conversations
- **Sub-agent architecture** for parallelizing complex tasks
- **Source map file** in npm package is what led to this repo
