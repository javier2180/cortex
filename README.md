# CortexPrism

<p align="center">
  <img src="docs/banner.svg" alt="CortexPrism" width="100%"/>
</p>

## Star History

<a href="https://www.star-history.com/?repos=CortexPrism%2Fcortex&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=CortexPrism/cortex&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=CortexPrism/cortex&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=CortexPrism/cortex&type=date&legend=top-left" />
 </picture>
</a>

> **The open-source AI agent operating system — autonomous agents with memory, tools, a web UI, and
> layered security, powered by Deno.**

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Deno 2.x](https://img.shields.io/badge/runtime-Deno%202.x-black)](https://deno.land)
[![Version](https://img.shields.io/badge/version-0.53.1-green)](CHANGELOG.md)
[![CI](https://github.com/CortexPrism/cortex/actions/workflows/ci.yml/badge.svg)](https://github.com/CortexPrism/cortex/actions/workflows/ci.yml)
[![Discord](https://img.shields.io/discord/1515847007372181584?color=5865F2&label=Discord&logo=discord&logoColor=white)](https://discord.gg/wYxbmQeWY3)

**CortexPrism** is a self-hosted, open-source **AI agent operating system** — an autonomous agent
runtime that turns any LLM into a capable digital agent. It provides persistent memory, a rich tool
ecosystem, sandboxed code execution, multi-agent orchestration, a full-featured web UI, and
enterprise-grade security — all running locally on your machine or server.

- Works with **30 LLM providers** out of the box (Anthropic, OpenAI, Gemini, Groq, Ollama, and more)
- Ships as a **single Deno binary** — no Docker required to get started
- **100% open source** — Apache 2.0 licensed, no telemetry, data stays on your machine

---

## Table of Contents

- [Features](#features)
- [Requirements](#requirements)
- [Quick Start](#quick-start)
- [CLI Reference](#cli-reference)
- [Configuration](#configuration)
- [Web UI](#web-ui)
- [Security Model](#security-model)
- [Plugin System](#plugin-system)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [License](#license)

---

## Features

### AI Providers & Model Routing

- **30 LLM providers** — Anthropic Claude, OpenAI GPT, Google Gemini, Mistral, Groq, DeepSeek,
  OpenRouter, xAI Grok, Together AI, AWS Bedrock, Cohere, Ollama (local models), Cerebras,
  Fireworks, Perplexity, NVIDIA NIM, Moonshot (Kimi), Novita AI, LM Studio, LiteLLM, Hugging Face,
  Alibaba (Qwen), Venice AI, Kilo AI, DeepInfra, Hyperbolic, MiniMax, Zhipu (GLM), Replicate,
  Cloudflare Workers AI
- **Multimodal input** — upload images and documents; native vision support for Anthropic and Google
  Gemini; PDF text auto-extracted for all providers
- **Model Quartermaster (MQM)** — intelligent model selection that learns which model performs best
  for each task type using a 6-signal prediction engine with adaptive EMA learning and three arbiter
  strategies (conservative / balanced / aggressive)
- **Model router** — RouteLLM-style cascade (cheapest-first escalation) and threshold
  (prompt-scoring) routing strategies

### Agent Capabilities

- **Interactive streaming chat** — CLI and Web UI with real-time streaming, session persistence, and
  session resume
- **Tool use with approval gates** — every tool call is reviewed by the security policy before
  execution; agents can request human approval for sensitive operations
- **Sub-agent orchestration** — agents can spawn 11 specialized child agents (Explorer, General,
  Planner, Coder, Researcher, Security, Debugger, Architect, DevOps, Data Analyst, UI/UX Designer)
  for parallel and delegated work
- **Per-turn reflection** — LLM self-assessment of confidence and quality after each response; meta-
  pattern consolidation over time
- **Voice pipeline** — speech-to-text (OpenAI Whisper), text-to-speech (OpenAI TTS / ElevenLabs),
  energy-based VAD, real-time audio streaming over WebSocket
- **Computer use** — GUI automation via virtual displays (Xvfb) with mouse, keyboard, and screenshot
  actions; Docker-isolated or native runtime
- **Code intelligence** — tree-sitter WASM code indexing across 14+ languages; call graph traversal,
  impact analysis, architecture extraction, and symbol search
- **Browser automation** — headless Playwright-powered browser for web navigation, interaction,
  screenshots, and accessibility snapshots

### Memory System

- **5-tier memory** — episodic (FTS5 full-text search), semantic (vector embeddings), procedural
  (skills), graph (knowledge graph), and reflection (learned patterns) with multi-strategy retrieval
  and decay scoring
- **Heuristic self-learning** — AI-driven memory improvement: access-tracking importance boosting,
  decay-slowing for frequent memories, co-occurrence graph relations, and 12-rule
  auto-categorization
- **Hybrid search** — keyword (FTS5 BM25) + vector (cosine similarity) search with time-decay
  scoring
- **Automatic memory injection** — relevant memories are injected into each turn's system prompt
- **Memory search CLI** — keyword, semantic, and hybrid search from the terminal
- **Memory health dashboard** — aggregated metrics for active/stale counts, decay, importance,
  access frequency, and graph stats

### Skills System

- **Self-learning skills** — the agent automatically extracts reusable procedural patterns from
  successful tool-call sequences and stores them as versioned, quality-scored skills
- **Skill lifecycle** — 6-state lifecycle (candidate → verified → released → degraded → deprecated →
  archived) with automatic health monitoring and staleness detection
- **Embedding-based retrieval** — skills are matched to user queries via cosine similarity over
  precomputed embeddings, with lexical fallback for cold starts
- **Skill deduplication** — similar skills are automatically detected and merged, preserving steps
  and bumping versions; LLM-extracted skills are deduplicated on creation
- **Dependency tracking** — skills can declare `depends_on` and `conflicts_with` relationships;
  deletion is blocked if other skills depend on the target
- **Trust tiering** — 4-tier system (1=LLM-extracted, 4=built-in vetted) gates agent skill exposure
- **Health system** — composite quality scores from utility, freshness, redundancy, and failure
  risk; automatic maintenance deprecates stale/low-quality skills
- **Tool-accessible** — `load_skill`, `skill_read`, `skill_write`
  (create/update/delete/merge/promote/deprecate)
- **Web UI & REST API** — full skill library management with lifecycle badges, trust stars,
  dependency graphs, health reports, and bulk operations
- **Skill SDK** — define skills as TypeScript modules (`src/skills/builtin/`) or markdown files in
  `.cortex/skills/<name>/SKILL.md` with YAML frontmatter

See [docs/SKILLS.md](docs/SKILLS.md) for the full reference.

### Code Intelligence (Codegraph)

- **Multi-language parsing** — tree-sitter WASM parser for 14+ languages (TS, JS, Python, Go, Rust,
  Java, Kotlin, C, C++, Ruby, PHP, Swift, Lua, Bash)
- **Call graph resolution** — 6-strategy call target resolution with import analysis across all
  supported languages
- **Code graph storage** — 14 node labels (CodeProject, CodeFile, CodeFunction, CodeClass, etc.), 18
  edge types (CALLS, IMPORTS, DEFINES, IMPLEMENTS, INHERITS, HTTP_CALLS, DECORATES, etc.)
- **6 agent tools** — code_index, code_search_symbol, code_trace_path, code_get_architecture,
  code_analyze_impact, code_list_projects
- **Web UI** — D3.js force-directed dependency graph with symbol search, impact analysis, and path
  tracing
- **Incremental sync** — file-hash-based change detection with chunked bulk insert and BFS-batched
  queries

### Built-in Tools

| Category          | Tools                                                                                           |
| ----------------- | ----------------------------------------------------------------------------------------------- |
| File system       | read, write, edit, patch, delete, rename, copy, move, list, tree, info, search, glob, undo/redo |
| Shell             | execute shell commands (sandboxed through policy validator)                                     |
| Web               | web_search, web_fetch, web_crawl, docs_search (Context7 docs), firecrawl (web scraping)         |
| Code execution    | sandboxed Docker/gVisor containers with resource limits; LLM auto-fix loop                      |
| Browser           | navigate, click, type, screenshot, snapshot, evaluate, wait (Playwright headless automation)    |
| GitHub            | PR creation/listing, issue tracking, repo browsing, git push                                    |
| Git workspace     | status, commit, push, pull, branch, clone                                                       |
| Voice             | speak, listen (STT/TTS agent tools)                                                             |
| Data & Util       | memory_note, memory_search, db_query, structured_extract, json_query, regex_utils, code_snippet |
| Environment       | env_manager (get/set variables), schedule (cron-based job scheduling)                           |
| Sandbox           | environment snapshots, workspace snapshots, dev env as code, bug reproduction studio            |
| Image & Vision    | image_analyze (multimodal image analysis via 18+ LLM providers)                                 |
| Sub-agents        | spawn typed child agents for parallel and delegated tasks                                       |
| Skills            | load_skill, skill_read, skill_write (create/update/delete/merge/promote/deprecate)              |
| Dashboard         | dashboard_manage — CRUD operations on dashboard widgets                                         |
| Nodes             | node_dispatch — dispatch tasks to remote distributed nodes                                      |
| Code Intelligence | code_index, code_search_symbol, code_trace_path, code_analyze_impact, code_get_architecture     |
| Computer Use      | screenshot, left_click, type, key, scroll, mouse_move, drag (GUI automation via Xvfb + xdotool) |

### Web UI & REST API

- **Built-in HTTP server** — `cortex server start` starts a WebSocket-powered chat UI on port 3000
- **Pages**: Chat, Dashboard, Editor, VCS, Sessions, Memory, Skills, Soul, Agents, Services, Nodes,
  Daemons, Automation, Channels, Tools, MCP, Codegraph, Policies, Vault, Settings, Quartermaster,
  Extensions, Analytics, Lens (Activity), Workflows, Eval Runner, Computer Use, Remote Agents,
  Sandbox, Prompt Lab, PKM, Metacognition, Voice, Tunnel, Projects, Teams, Users
- **File upload** — drag-and-drop or click to attach PDFs, images, and documents in chat
- **REST API** — full HTTP API for sessions, memory, jobs, git, GitHub, and code execution
- **Session persistence** — page refresh resumes the active session (full history preserved)

### Security (Parallax Model + LLM Supervisor)

- **Policy validator** — every tool call is evaluated against regex allow/deny rules before
  execution
- **Dynamic tool permission grant** — per-task permission evaluation with risk profiles and
  guardrails (readOnly, restrictedPaths, allowedDomains, requireConfirmation)
- **Tool approval workflow** — structured approval pipeline with auto-approve thresholds, webhook
  notifications, and 5-minute timeouts
- **LLM supervisor** — sensitive data access (memory, databases, screenshots) requires approval from
  a fast LLM supervisor model (Gemini 2.0 Flash, GPT-4o Mini) with decision caching and human
  escalation for uncertain cases
- **Data classification** — automatic sensitivity detection (SECRET/SENSITIVE/NORMAL/PUBLIC) based
  on pattern matching (passwords, API keys, PII, confidential markers); all existing data backfilled
  on first run
- **DLP Guard** — 22-scanner data loss prevention scanning all agent outputs for sensitive data (API
  keys, credentials, PII, PHI, PCI); supports monitor/redact/block action levels
- **AI Guardrails** — pluggable content safety middleware with 5 built-in classifiers: prompt
  injection (10 patterns), PII leakage, harmful code, excessive length, and shell injection
- **Session isolation** — multi-tenant data isolation with path-based and environment-variable
  gating across strict, permissive, and shared modes
- **Human approval flows** — CLI color-coded prompts and Web UI modal for sensitive access requests,
  with AI supervisor reasoning and sample data preview
- **Temporary grants** — approved access cached per session to prevent approval fatigue while
  maintaining security
- **AES-256-GCM vault** — encrypted credential storage with PBKDF2 key derivation
- **Default deny rules** — ships with protection against `rm -rf /`, fork bombs, direct disk writes
- **Activity** — full audit log of all sessions, tool calls, LLM calls, policy decisions, and
  security approvals with cost tracking See
  [docs/SECURITY_SUPERVISOR.md](docs/SECURITY_SUPERVISOR.md) for the full architecture.

### Computer Use (GUI Automation)

- **Virtual display** — X11 virtual framebuffer (Xvfb) lifecycle management with multi-display
  support
- **Mouse control** — coordinate-based movement, left/right/middle clicks, double/triple clicks,
  drag
- **Keyboard control** — text typing with configurable delays, key combinations, key holding
- **Screenshot capture** — PNG/JPEG output via scrot, ImageMagick, or xwd with automatic fallback
- **15 agent actions** — screenshot, click, type, key, scroll, wait, mouse_move, drag, and more
- **Security** — all actions gated through policy validator with user approval; sensitive data
  auto-blocked
- **Docker support** — pre-built Ubuntu 22.04 image with XFCE, Firefox, Chromium, LibreOffice
- **Requirements** — `xvfb`, `xdotool`, `scrot` (Linux-only)

See [docs/computer-use/README.md](docs/computer-use/README.md) for the full guide.

### Ops & Extensibility

- **Multi-user collaboration** — users, teams, API tokens with SHA-256 hashing, resource scoping,
  federation between instances
- **Distributed swarm** — cross-instance agent coordination via A2A protocol with fleet topology,
  resource aggregation, and directive dispatch
- **Scheduled jobs** — SQLite-persisted cron with automatic retry
- **Daemon supervisor** — manages validator, executor, and scheduler processes with exponential
  backoff restart
- **Plugin system** — WASM and Deno module plugins with sandboxed permissions
- **MCP Gateway** — enterprise MCP server management with rate limiting, health checks, audit
  logging
- **A2A Protocol Bridge** — Google Agent2Agent (A2A) v1.0 protocol for cross-framework agent
  collaboration with JSON-RPC 2.0 server/client, SSE streaming, and tool wrapping
- **Memori Checkpointing** — persistent agent state serialization and restore for survival across
  restarts, crashes, and context window resets
- **Supply chain integrity** — plugin verification with SHA-256 hash checking, signature
  verification, author reputation scoring, and malware pattern scanning
- **Dependency guardian** — continuous CVE monitoring, license enforcement, and remediation
  suggestions across 6 package ecosystems
- **AgentLint** — automated auditing of agent configs, tools, plugins, and prompts with 33+ checks
- **Auto-update** — `cortex self update` supports source mode and signed binary mode with SHA-256
  and optional GPG verification
- **Desktop app** — Tauri-based desktop wrapper (macOS, Windows, Linux)

---

## Requirements

| Requirement                   | Notes                                                                            |
| ----------------------------- | -------------------------------------------------------------------------------- |
| [Deno 2.x](https://deno.land) | Required — the installer handles this automatically                              |
| Docker                        | Optional — needed for sandboxed code execution; subprocess fallback is available |
| macOS, Linux, or Windows      | All platforms supported                                                          |

---

## Quick Start

### Option 1: One-line installer (recommended)

**macOS / Linux:**

```bash
curl -fsSL https://cortexprism.io/install.sh | bash
```

**Windows (PowerShell):**

```powershell
irm https://cortexprism.io/install.ps1 | iex
```

The installer: checks for / installs Deno, clones Cortex to `~/.cortex`, creates the `cortex` CLI
alias, and runs database migrations. After install, run `cortex setup` to configure your first LLM
provider.

### Option 2: Manual clone

```bash
git clone https://github.com/CortexPrism/cortex.git ~/.cortex
cd ~/.cortex
deno task migrate
deno run --allow-all src/main.ts setup
```

Add `cortex` to your PATH by appending to your shell profile:

```bash
echo 'alias cortex="deno run --allow-all ~/.cortex/src/main.ts"' >> ~/.bashrc
source ~/.bashrc
```

### Option 3: Pre-compiled binary

Download the latest binary from the [Releases page](https://github.com/CortexPrism/cortex/releases).
All binaries include SHA-256 checksums and optional GPG signatures.

### First run

```bash
cortex setup        # Interactive setup wizard — choose provider, enter API key
cortex agent chat         # Start your first chat session
cortex server start        # Open the Web UI at http://127.0.0.1:3000
```

---

## CLI Reference

```
cortex <command>

Commands:
  agent             Agent commands (chat, exec, tui, sessions, eval, reflect, lint, voice)
  setup             Re-run the setup wizard
  server start      Start the HTTP + WebSocket server with Web UI
  daemon            Manage background processes (validator, executor, scheduler)
  sandbox           Code execution in sandboxed environment
  memory            Search and manage memory
  jobs              Manage scheduled jobs
  vault             Encrypted credential vault (store / get / list / delete)
  policy            Security policy rules (list / add / remove / check)
  db migrate        Initialise or migrate all databases
  self update       Check for and apply updates
  config            View and edit configuration
  git               Git workspace operations
  github            GitHub integration (PRs, issues, repos)
  mqm               Model Quartermaster stats and configuration
  qm                Quartermaster tool orchestration stats
  models            List and configure LLM models
  soul              Manage agent identity and personality templates
  plugins           Install and manage plugins
  marketplace       Browse and install from the plugin/agent marketplace
  log               View and manage logging configuration
  service           Manage micro-services (start, stop, install, uninstall)
  node              Manage distributed nodes
  hooks             Manage pipeline hooks
  triggers          Manage event triggers
  channels          Manage channel adapters (Discord, etc.)
  mcp               MCP server commands (serve, stdio, chrome, a2a)
  desktop           Desktop automation
  workflow          Workflow engine operations
  projects          Project management
  swarm             Distributed agent swarm (init, nodes, topology, report)
  login             Authenticate with username/password or API token
  logout            End current authenticated session
  whoami            Show current authenticated user
  users             User management (list, create, disable, enable)
  teams             Team management (list, create)
  compliance        Compliance policy management
  debug             Debug utilities
  memori            Memori checkpointing and persistence
  import            Tool output import
  tunnel            Tunnel management
  run               Execute a task via CLI
  update            Update CortexPrism
  migrate           Run pending