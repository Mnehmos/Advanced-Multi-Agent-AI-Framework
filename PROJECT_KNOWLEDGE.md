# Advanced Multi-Agent AI Framework - Knowledge Base Document

## Quick Reference

| Property | Value |
|----------|-------|
| **Repository** | https://github.com/Mnehmos/mnehmos.multi-agent.framework |
| **Primary Language** | YAML / Markdown |
| **Project Type** | Framework |
| **Status** | Active |
| **Last Updated** | 2025-12-29 |

## Overview

The Advanced Multi-Agent AI Framework is a structured, production-ready multi-agent coordination system for AI-powered development workflows. It provides specialized AI agent modes (Orchestrator, Architect, Planner, Code, Debug, etc.) that work together through clear contracts, deterministic execution, and traceable task flows. The framework is platform-agnostic and works with Roo Code, Claude Code, Cursor, GitHub Copilot, and custom runtimes. It enables teams to coordinate complex development tasks using TDD workflows, OODA loop patterns, and boomerang-style task delegation.

## Architecture

### System Design

The framework implements a multi-agent coordination pattern where specialized agents collaborate through an Orchestrator that manages task delegation and workflow state. The architecture is built on three core patterns:

1. **Multi-Agent Coordination**: Specialized agents (Orchestrator, Architect, Planner, Code, Debug, Ask, Memory, Deep Research) work together with clear role boundaries defined in YAML configuration files.

2. **TDD Workflow Phases**: Development follows Red (write failing tests) → Green (minimal implementation) → Blue (refactor) cycle phases, each implemented as separate agent modes.

3. **Boomerang Task Returns**: Every delegated task returns structured payloads containing status, files changed, tests run, and summary for full traceability.

The framework does not execute code directly; instead, it provides configuration templates and instruction sets that IDE/agent runtimes interpret to enable multi-agent workflows.

### Key Components

| Component | Purpose | Location |
|-----------|---------|----------|
| Custom Modes YAML | Agent mode definitions (Orchestrator, Architect, Planner, TDD phases, etc.) | `templates/custom_modes.yaml` |
| Slash Commands YAML | Declarative command definitions for workflows (/plan, /scope, /build, etc.) | `templates/slash-commands.yaml` |
| Universal AGENTS.md | Platform-agnostic agent instructions for any IDE | `templates/universal/AGENTS.md` |
| Meet the Team | Detailed documentation for each agent role | `meet-the-team/` |
| Slash Commands Docs | Per-command behavior specifications | `slash-commands/` |
| Tool Instructions | MCP tool integration guides | `templates/tools/` |
| Tool-Enabled Templates | Pre-configured templates with MCP tools | `templates/tool-enabled/` |
| IDE-Specific Templates | Roo Code, Claude Code, Cursor, Copilot configurations | `templates/claude-code/`, `templates/cursor/`, etc. |
| Website | Astro-based documentation site | `website/` |
| Custom Instructions | Shared behavioral contracts for all modes | `templates/custom-instructions-for-all-modes.md` |
| Prompt Enhancement | Templates for improving agent prompts | `templates/enhance-prompt-template.md` |

### Data Flow

```
User Goal
    ↓
Orchestrator Mode (decomposes into task map)
    ↓
Task Delegation (assigns to specialist modes with workspace scope)
    ↓
Worker Modes (Red/Green/Blue/Code/Debug execute within scope)
    ↓
Boomerang Return (structured payload: status, files, tests, summary)
    ↓
Orchestrator (validates results, updates task map, continues workflow)
    ↓
User (receives completion summary with audit trail)
```

## API Surface

### Public Interfaces

#### Mode: `orchestrator`
- **Purpose**: Project coordination and multi-step workflow management
- **Parameters**:
  - User goal/objective (string): High-level task description
  - Context (object): Current project state, available modes, constraints
- **Returns**: Task Maps with structured subtasks (task_id, mode, scope, dependencies, acceptance criteria)
- **Constraints**: MUST NOT directly edit files; delegates all implementation work

#### Mode: `red-phase` (TDD Red)
- **Purpose**: Write failing tests before implementation
- **Parameters**:
  - Task specification (object): Scope, file patterns, expected behavior
  - workspace_path (string): Test directory to operate in
- **Returns**: Boomerang payload with test files created, failure verification
- **Constraints**: ONLY modifies test files; MUST verify tests fail

#### Mode: `green-phase` (TDD Green)
- **Purpose**: Minimal implementation to make tests pass
- **Parameters**:
  - Task specification (object): Scope, file patterns, test requirements
  - workspace_path (string): Implementation directory
- **Returns**: Boomerang payload with implementation files, all tests passing
- **Constraints**: ONLY modifies implementation files; MUST run and pass all tests

#### Mode: `blue-phase` (TDD Blue)
- **Purpose**: Refactor code while maintaining green tests
- **Parameters**:
  - Task specification (object): Scope, refactoring goals
  - workspace_path (string): Files to refactor
- **Returns**: Boomerang payload with refactored files, quality improvements
- **Constraints**: Tests MUST remain green throughout

#### Mode: `code`
- **Purpose**: Advanced implementation, refactoring, and optimization
- **Parameters**:
  - Task specification (object): Complex technical work requiring high skill
  - workspace_path (string): Implementation scope
- **Returns**: Boomerang payload with changes, tests, validation steps
- **Constraints**: Operate within assigned scope; preserve existing contracts

#### Mode: `architect`
- **Purpose**: System design, architecture decisions, ADRs
- **Parameters**:
  - Design goals (string): What needs to be architected
  - Constraints (object): Technical/business constraints
- **Returns**: Architecture documents, ADRs, design specifications
- **Constraints**: Does not implement; produces design artifacts only

#### Mode: `planner`
- **Purpose**: Requirements analysis and task map generation
- **Parameters**:
  - Project goals (string): What needs to be planned
  - Timeline/complexity (string): Planning depth required
- **Returns**: Task Maps with phases, dependencies, acceptance criteria
- **Constraints**: Planning only; no code changes

#### Mode: `debug`
- **Purpose**: Diagnostics, root cause analysis, issue investigation
- **Parameters**:
  - Issue description (string): Bug or failure to investigate
  - Reproduction context (object): Environment, steps, logs
- **Returns**: Root cause analysis, reproduction steps, fix recommendations
- **Constraints**: Read-only investigation; proposes fixes but doesn't implement

#### Mode: `ask`
- **Purpose**: Information retrieval and explanation
- **Parameters**:
  - Question (string): What needs to be explained
  - Context (object): Relevant project/domain context
- **Returns**: Concise explanation with contract references
- **Constraints**: Read-only; recommends which mode should act next

#### Mode: `memory`
- **Purpose**: Knowledge management and documentation organization
- **Parameters**:
  - Knowledge to organize (object): Decisions, artifacts, references
  - Structure requirements (string): How to organize
- **Returns**: Structured documentation with task/run references
- **Constraints**: Manages documentation; does not implement features

#### Mode: `deep-research-agent`
- **Purpose**: Multi-source research and technical analysis
- **Parameters**:
  - Research topic (string): What to investigate
  - Depth (string): Surface/comprehensive/exhaustive
- **Returns**: Rigorous research report with citations
- **Constraints**: Research only; outputs consumable by other modes

### Slash Commands

The framework provides 20+ standardized slash commands across 5 categories:

**Project Management**: `/plan`, `/scope`, `/assign`, `/status`, `/review`, `/merge`
**Architecture**: `/design`, `/diagram`, `/pattern`, `/validate`
**Development**: `/build`, `/test`, `/refactor`, `/optimize`, `/debug`
**Research**: `/research`, `/analyze`, `/compare`, `/synthesis`
**Framework**: `/mode`, `/config`, `/template`, `/workflow`
**Utility**: `/help`, `/docs`, `/logs`, `/export`

Each command is defined in `templates/slash-commands.yaml` with:
- Enabled status, aliases, description
- Category, permissions required
- Implementation hints and SPARC alignment

### Configuration

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `customModes[].slug` | string | (required) | Unique mode identifier (e.g., "orchestrator") |
| `customModes[].name` | string | (required) | Display name with emoji (e.g., "🔄 Orchestrator") |
| `customModes[].roleDefinition` | string | (required) | Core purpose and behaviors of the mode |
| `customModes[].whenToUse` | string | (required) | Guidance on when to activate this mode |
| `customModes[].groups` | array | `["read"]` | Tool permissions (read, edit, command, browser, mcp) |
| `customModes[].customInstructions` | string | `""` | Mode-specific contract details |
| `commands[].enabled` | boolean | `true` | Whether slash command is active |
| `commands[].aliases` | array | `[]` | Alternative names for the command |
| `commands[].category` | string | (required) | Command grouping (project-management, architecture, etc.) |
| `commands[].permissions` | array | `["read"]` | Required tool access for command execution |

## Usage Examples

### Basic Usage

```yaml
# Configure a new multi-agent project with Roo Code
# 1. Copy templates/custom_modes.yaml to your project
# 2. Copy templates/custom-instructions-for-all-modes.md
# 3. Configure Roo to load these custom modes

# Example custom_modes.yaml snippet:
customModes:
  - slug: orchestrator
    name: "🔄 Orchestrator"
    roleDefinition: |
      You are the Orchestrator for the Advanced Multi-Agent AI Framework.
      Your purpose:
      - Plan and coordinate work across modes
      - Decompose high-level goals into atomic subtasks
      - Enforce boomerang-style structured returns
    whenToUse: >
      Use when tasks need to be broken down, scheduled, delegated, and verified.
    groups:
      - read
      - browser
      - command
      - mcp
```

```bash
# Quick start with universal AGENTS.md (works in any IDE)
cp templates/universal/AGENTS.md /path/to/your/project/AGENTS.md

# For Claude Code specifically
cp templates/claude-code/CLAUDE.md /path/to/your/project/.claude/CLAUDE.md

# For Cursor IDE
cp templates/cursor/rules/_global.mdc /path/to/your/project/.cursorrules/_global.mdc
```

### Advanced Patterns

```yaml
# TDD workflow with three-phase delegation
# Orchestrator creates task map:

# Task 1: Red Phase
task_id: "implement-auth-red"
mode: "red-phase"
workspace_path: "tests/auth/"
file_patterns: ["*.test.ts"]
objective: "Write failing tests for authentication module"
acceptance_criteria:
  - "Tests cover login, logout, token refresh"
  - "Tests fail with clear error messages"
  - "npm test -- auth returns failures"

# Task 2: Green Phase (depends on Task 1)
task_id: "implement-auth-green"
mode: "green-phase"
workspace_path: "src/auth/"
file_patterns: ["*.ts"]
dependencies: ["implement-auth-red"]
objective: "Implement minimal auth to pass tests"
acceptance_criteria:
  - "All auth tests pass"
  - "No features beyond test requirements"
  - "Functions registered in registry.ts"

# Task 3: Blue Phase (depends on Task 2)
task_id: "implement-auth-blue"
mode: "blue-phase"
workspace_path: "src/auth/, tests/auth/"
file_patterns: ["*.ts", "*.test.ts"]
dependencies: ["implement-auth-green"]
objective: "Refactor auth for quality and polish"
acceptance_criteria:
  - "All tests remain green"
  - "Code follows DRY principles"
  - "Output formatted with Markdown and emojis"
```

```bash
# Using slash commands in your IDE
/plan "Build user authentication system" --depth=comprehensive
# → Orchestrator generates full task map with phases

/scope https://github.com/org/repo/issues/123 --components=backend
# → Deep Scope mode analyzes issue and creates scope document

/assign task-auth-001 --mode=red-phase --priority=high
# → Delegates task to Red Phase mode with structured spec

/build --target=production --run-tests --quality-gates
# → Coordinates build workflow with validation steps

/debug "Login fails with 401 on valid credentials"
# → Debug mode investigates and proposes fix
```

## Dependencies

### Runtime Dependencies

This framework has no runtime dependencies as it is a configuration/template framework. The templates generate YAML and Markdown configuration files consumed by IDE/agent runtimes.

The website component has the following dependencies:

| Package | Version | Purpose |
|---------|---------|---------|
| astro | ^5.16.6 | Static site generator for documentation website |
| tailwindcss | ^4.1.18 | CSS framework for website styling |
| @tailwindcss/vite | ^4.1.18 | Vite integration for Tailwind CSS |

### Development Dependencies

None - this is a pure template/configuration framework with no build process at the framework level. The website has its own dev dependencies managed separately.

## Integration Points

### Works With

| Project | Integration Type | Description |
|---------|-----------------|-------------|
| mnehmos.ooda.mcp | Extension | Full computer control (CLI, files, screen, keyboard) via MCP tools - 62 tools available |
| mnehmos.synch.mcp | Extension | Agent memory bank, context sync, bug tracking, handoffs - ~17 tools |
| mnehmos.index-foundry.mcp | Extension | RAG indexing, vector search, deployable projects - ~35 tools |
| mnehmos.arxiv.mcp | Extension | Academic paper search and PDF extraction - 4 tools |
| mnehmos.trace.mcp | Extension | Schema tracing, producer/consumer validation - 11 tools |
| mnehmos.chatrpg.game | Extension | D&D 5e mechanics for AI Dungeon Masters - 30+ tools |

All MCP integrations are optional add-ons. Tool instruction templates are provided in `templates/tools/` with pre-configured bundles in `templates/tool-enabled/`.

### External Services

No external services required. The framework is entirely local configuration files. Optional MCP tools may require external APIs (e.g., OpenAI for embeddings in Index Foundry), but those are tool-specific, not framework requirements.

## Development Guide

### Prerequisites

- Any IDE or agent runtime supporting:
  - Multiple modes/roles with distinct instructions
  - Atomic tool execution
  - File-scoped deterministic edits
- Supported environments include:
  - Roo Code (full multi-mode support)
  - Claude Code (system instructions)
  - Cursor IDE (rules in MDC format)
  - GitHub Copilot (custom instructions)
  - Any custom MCP-compatible runtime

### Setup

```bash
# Clone the repository
git clone https://github.com/Mnehmos/mnehmos.multi-agent.framework
cd mnehmos.multi-agent.framework

# No installation required - this is a template framework
# Copy templates to your project as needed

# For Roo Code: Copy custom_modes.yaml and custom-instructions
cp templates/custom_modes.yaml /path/to/your/project/.roo/
cp templates/custom-instructions-for-all-modes.md /path/to/your/project/

# For universal IDE support: Copy AGENTS.md
cp templates/universal/AGENTS.md /path/to/your/project/

# For IDE-specific setup, see templates/quick-start.md
```

### Running Locally

```bash
# This framework doesn't "run" - it provides configuration templates

# To develop the documentation website:
cd website
npm install
npm run dev
# Website available at http://localhost:4321

# To build website for production:
npm run build
# Output in website/dist/
```

### Testing

This framework has no tests as it is a configuration/template repository. Validation is done through:
- Manual review of YAML/Markdown files
- Testing configurations in target IDE environments (Roo, Claude Code, Cursor, etc.)
- User feedback and issue reports

### Building

```bash
# No build process for the framework itself

# To build the documentation website:
cd website
npm run build

# Output location: website/dist/
# Deploy to GitHub Pages or any static host
```

## Maintenance Notes

### Known Issues

1. The framework uses platform-agnostic templates that require manual adaptation for each IDE/runtime environment
2. Slash command parsing and routing must be implemented separately in each target runtime (reference implementation in `slash-commands/core/` is example only)
3. Boomerang task return payloads are conceptual/structural - actual JSON serialization depends on runtime capabilities
4. Some modes reference MCP tools that must be installed separately and configured in the user's MCP client

### Future Considerations

1. Consider developing runtime adapters/plugins for popular IDEs to automate template configuration
2. Evaluate creating a CLI tool to initialize projects with framework templates
3. Explore standardizing boomerang payload format across different runtime environments
4. Consider adding example projects demonstrating framework usage in real-world scenarios
5. Evaluate creating a schema validation tool for custom_modes.yaml to catch configuration errors early

### Code Quality

| Metric | Status |
|--------|--------|
| Tests | None (configuration/template repository) |
| Linting | None (YAML/Markdown content) |
| Type Safety | N/A (no code execution) |
| Documentation | Comprehensive - README, per-mode docs, per-command docs, template guides |

---

## Appendix: File Structure

```
mnehmos.multi-agent.framework/
├── templates/
│   ├── custom_modes.yaml           # Core mode definitions (Orchestrator, TDD phases, specialists)
│   ├── slash-commands.yaml         # Declarative slash command specifications
│   ├── custom-instructions-for-all-modes.md  # Shared behavioral contracts
│   ├── enhance-prompt-template.md  # Prompt improvement patterns
│   ├── quick-start.md              # Setup guide for different IDEs
│   ├── universal/
│   │   └── AGENTS.md               # Platform-agnostic agent instructions
│   ├── claude-code/
│   │   └── CLAUDE.md               # Claude Code system instructions
│   ├── cursor/
│   │   └── rules/_global.mdc       # Cursor IDE rules format
│   ├── copilot/
│   │   └── copilot-instructions.md # GitHub Copilot custom instructions
│   ├── tools/
│   │   ├── README.md               # MCP tools overview
│   │   ├── ooda-mcp.md             # OODA loop tool instructions
│   │   ├── synch-mcp.md            # Synch memory bank instructions
│   │   ├── index-foundry-mcp.md    # RAG indexing instructions
│   │   ├── arxiv-mcp.md            # arXiv search instructions
│   │   ├── trace-mcp.md            # Schema tracing instructions
│   │   └── chatrpg-mcp.md          # D&D mechanics instructions
│   └── tool-enabled/
│       ├── README.md               # Guide to tool-enabled templates
│       └── custom-instructions-with-tools.md  # Pre-configured with all tools
├── meet-the-team/
│   ├── orchestrator.md             # Orchestrator role detailed spec
│   ├── architect.md                # Architect role detailed spec
│   ├── planner.md                  # Planner role detailed spec
│   ├── code.md                     # Code specialist role spec
│   ├── debug.md                    # Debug specialist role spec
│   ├── ask.md                      # Ask/information role spec
│   ├── memory.md                   # Memory/knowledge management spec
│   ├── deep-research-agent.md      # Deep research role spec
│   └── ...                         # Additional specialist roles
├── slash-commands/
│   ├── README.md                   # Slash commands user guide
│   ├── plan.md                     # /plan command specification
│   ├── scope.md                    # /scope command specification
│   ├── assign.md                   # /assign command specification
│   ├── build.md                    # /build command specification
│   ├── test.md                     # /test command specification
│   ├── debug.md                    # /debug command specification
│   └── ...                         # Additional command specs
├── website/
│   ├── package.json                # Website dependencies (Astro, Tailwind)
│   ├── astro.config.mjs            # Astro configuration
│   ├── src/                        # Website source files
│   └── dist/                       # Built website output
├── .github/
│   └── workflows/                  # GitHub Actions for website deployment
├── README.md                       # Main project documentation
├── CONTRIBUTING.md                 # Contribution guidelines
├── CODE_OF_CONDUCT.md              # Community code of conduct
├── LICENSE                         # MIT License
└── PROJECT_KNOWLEDGE.md            # This document
```

---

*Generated by Project Review Orchestrator | 2025-12-29*
*Source: https://github.com/Mnehmos/mnehmos.multi-agent.framework*
