# Opencode Configuration

This repository contains the personalized configuration and custom extensions for the [Opencode](https://opencode.ai) AI development environment. It defines custom agents, commands, skills, and behavior rules to tailor the AI's capabilities for automated development, QA testing, and project planning.

## Directory Structure

```text
~/.config/opencode/
├── AGENTS.md           # Global instructions, memory management, and agent routing rules
├── opencode.json       # Main configuration file (plugins, MCP, permissions)
├── agents/             # Custom subagent definitions
│   ├── docs-writer.md       # Agent for technical documentation
│   └── playwright-tester.md # Agent for QA and Playwright automation
├── commands/           # Custom executable commands
│   ├── execute.md           # Task execution and verification workflow
│   └── fleshout.md          # Roadmap planning and task breakdown workflow
├── skills/             # Custom AI skills
│   └── playwright-cli/      # Skill for browser automation and test generation
└── package.json        # Plugin dependencies (e.g., @opencode-ai/plugin)
```

## Key Features

### 1. Global Agent Instructions (`AGENTS.md`)
Enforces standard operating procedures across all sessions:
- Verifies clean Git status before initiating workflows.
- Routes specific tasks to specialized subagents (`@docs-writer` for docs, `@playwright-tester` for UI testing).
- Integrates continuous memory management (`mnemosyne`) using `memory_recall` and `memory_store` to maintain cross-session context and personal preferences.

### 2. Custom Agents
- **`@docs-writer`**: A specialized subagent focused solely on creating and maintaining technical documentation, READMEs, and guides. It is configured to safely write `.md` files even from read-only contexts.
- **`@playwright-tester`**: An expert QA Automation subagent equipped with the `playwright-cli` skill. It analyzes UI code, writes semantic tests, runs verifications, and manages end-to-end browser automation.

### 3. Custom Commands
- **`execute`**: Driven by the `build` agent, this command sequentially processes task specifications or roadmap items, verifying acceptance criteria, and explicitly enforcing linting, testing, and UI verification.
- **`fleshout`**: Driven by the `docs-writer` agent, this command breaks down high-level project roadmap items into detailed, actionable tasks with clear acceptance criteria and dependencies.

### 4. Custom Skills
- **`playwright-cli`**: A deeply integrated skill allowing agents to automate browser interactions without writing boilerplate code. It supports navigation, form filling, snapshot analysis, session management, mock networking, DevTools tracing, and automatic test generation.

### 5. Security & Permissions (`opencode.json`)
Overrides default agent permissions to maintain a secure automated environment:
- **Git Protections**: Explicitly denies automated `git commit` and `git push` across all agents to prevent unapproved repository modifications.
- **Bash Execution**: Extends timeout limits for long-running build operations while defaulting most bash commands to require user approval.

### 6. Integrations & Plugins
Configures Model Context Protocol (MCP) integrations:
- **Context7**: Local MCP for code understanding.
- **Tavily**: Remote MCP for web search and research.
- Also integrates local plugins (e.g. `mnemosyne` memory plugin) and the `opencode-notifier` for state notifications.

## Installation & Setup

If you are cloning this configuration to a new machine:

1. Clone or copy this repository to `~/.config/opencode`.
2. Install the necessary node dependencies using Bun or npm:
   ```bash
   bun install
   # or
   npm install
   ```
3. Ensure the required CLI tools (like `playwright-cli`, `npx`, etc.) are available in your global environment.
4. Restart your Opencode session to apply the new configuration.

## Usage Guide

- **Triggering Commands**: You can invoke custom commands directly in your Opencode prompt (e.g., `/execute` or `/fleshout`).
- **Invoking Agents**: Mention `@docs-writer` or `@playwright-tester` in your prompt to delegate tasks specifically to them.
- **Memory Context**: The system will automatically recall relevant global memories at the start of a session. You can explicitly ask the AI to "remember this" to store custom preferences.
- **Git Workflow**: Since automated commits are blocked by default, the AI will stage files and present a `git status` summary. You must manually execute or approve the final commit.
