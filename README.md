# Agentic Nervous System

> Your LLM is a brain in a jar. Give it a nervous system.

A biological architecture for artificial minds. Sensation, reflex, memory, and action—organized into coherent loops that turn chatbots into organisms.

🌐 **Website**: UNDER CONSTRUCTION! [mnehmos.github.io/mnehmos.multi-agent.framework](https://mnehmos.github.io/mnehmos.multi-agent.framework)

📖 **Blog Post**: COMING SOON! [From Chatbot to Organism](https://mnehmos.github.io/blog/from-chatbot-to-organism)

---

## MCP Ecosystem

| Server | Layer | Description |
|--------|-------|-------------|
| [![OODA MCP](https://img.shields.io/badge/🦾_OODA_MCP-Somatic-f59e0b)](https://github.com/Mnehmos/mnehmos.ooda.mcp) | Somatic | 62 tools for computer automation: files, screen, keyboard, mouse, batch ops |
| [![Synch MCP](https://img.shields.io/badge/💭_Synch_MCP-Autonomic-3b82f6)](https://github.com/Mnehmos/mnehmos.synch.mcp) | Autonomic | Memory persistence, agent handoff, file locks, bug tracking |
| [![Index Foundry](https://img.shields.io/badge/📚_Index_Foundry-Autonomic-3b82f6)](https://github.com/Mnehmos/mnehmos.index-foundry.mcp) | Autonomic | RAG pipelines, document Q&A, semantic search |
| [![Trace MCP](https://img.shields.io/badge/⚡_Trace_MCP-Reflex-ef4444)](https://github.com/Mnehmos/mnehmos.trace.mcp) | Reflex | Schema validation, contract enforcement, type scaffolding |
| [![arXiv MCP](https://img.shields.io/badge/📄_arXiv_MCP-Central-a855f7)](https://github.com/Mnehmos/mnehmos.arxiv.mcp) | Central | Academic paper search for Deep Research mode |
| [![ChatRPG](https://img.shields.io/badge/🎮_ChatRPG-Game-10b981)](https://github.com/Mnehmos/mnehmos.chatrpg.game) | Game Engine | D&D 5e combat, characters, encounters |

---

## The Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                    CENTRAL (BRAIN) - Cognition                      │
│    Orchestrator • Architect • Planner • Ask • Deep Research        │
│         ↑ Receives sensory input   ↓ Sends motor commands          │
├─────────────────────────────────────────────────────────────────────┤
│                   SOMATIC (BODY) - Voluntary Action                 │
│         Red Phase • Green Phase • Blue Phase • Code • Debug        │
│              ↑ Tool results   ↓ Tool invocations                   │
├─────────────────────────────────────────────────────────────────────┤
│                AUTONOMIC (SUBCONSCIOUS) - Background                │
│              Memory • Synch MCP • Index Foundry MCP                 │
│           State persistence, retrieval, agent coordination         │
├─────────────────────────────────────────────────────────────────────┤
│                   REFLEX (SPINAL CORD) - Immediate                  │
│                Trace MCP • Schema Validation • Contracts            │
│              Rejects bad inputs before cognition                    │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Quick Start

### 1. Copy the baseline templates

```bash
# Create .roo directory in your project
mkdir -p .roo

# Copy templates
cp templates/custom_modes.yaml .roo/
cp templates/universal/AGENTS.md .
```

**Files to copy:**
- [`templates/custom_modes.yaml`](templates/custom_modes.yaml) → `.roo/custom_modes.yaml`
- [`templates/universal/AGENTS.md`](templates/universal/AGENTS.md) → `AGENTS.md` (project root)

### 2. Add global instructions

Copy [`templates/custom-instructions-for-all-modes.md`](templates/custom-instructions-for-all-modes.md) content to your IDE's global instructions field.

### 3. (Optional) Add MCP tools

Install tools to extend capabilities. See [`templates/tools/`](templates/tools/) for integration docs:

| Tier | Tool | Layer | Integration Guide |
|------|------|-------|-------------------|
| 2 | [OODA MCP](https://github.com/Mnehmos/mnehmos.ooda.mcp) | Somatic | [`ooda-mcp.md`](templates/tools/ooda-mcp.md) |
| 3 | [Synch MCP](https://github.com/Mnehmos/mnehmos.synch.mcp) | Autonomic | [`synch-mcp.md`](templates/tools/synch-mcp.md) |
| 3 | [Index Foundry](https://github.com/Mnehmos/mnehmos.index-foundry.mcp) | Autonomic | [`index-foundry-mcp.md`](templates/tools/index-foundry-mcp.md) |
| 4 | [Trace MCP](https://github.com/Mnehmos/mnehmos.trace.mcp) | Reflex | [`trace-mcp.md`](templates/tools/trace-mcp.md) |

---

## Template Tiers

Progressive enhancement—start simple, add capabilities as needed:

| Tier | Components | What You Get |
|------|------------|--------------|
| **0** | Baseline templates | Toolless coordination with 13 modes |
| **1** | + Native tools | File I/O, command execution |
| **2** | + OODA MCP | Full computer automation (62 tools) |
| **3** | + Synch + Index Foundry | Memory persistence + RAG |
| **4** | + Trace MCP | Schema validation layer |

Each tier preserves all contracts from previous tiers.

---

## The 13 Modes

### Central (Brain)
| Mode | Purpose |
|------|---------|
| 🔄 Orchestrator | Task decomposition, delegation, integration |
| 🏛️ Architect | System design, ADRs, contracts |
| 📋 Planner | Task maps, backlogs, dependencies |
| ❓ Ask | Clarification, research synthesis |
| 🔎 Deep Research | Multi-source investigation |
| 🔬 Deep Scope | Issue scoping |

### Somatic (Body)
| Mode | Purpose |
|------|---------|
| 🔴 Red Phase | Write failing tests |
| 🟢 Green Phase | Minimal implementation |
| 🔵 Blue Phase | Refactor with green tests |
| 💻 Code | Complex implementation |
| 🐛 Debug | Root cause analysis |

### Autonomic (Subconscious)
| Mode | Purpose |
|------|---------|
| 📁 Memory | Documentation, knowledge management |
| 🗄️ Index Foundry | RAG pipeline specialist |

---

## Core Contracts

### OODA Loop

All work follows Observe → Orient → Decide → Act:

1. **Observe** 👁️ — Read the task, scan files, check tests
2. **Orient** 🧭 — Map what exists vs what's needed
3. **Decide** 🎯 — Choose approach with lowest risk
4. **Act** ⚡ — Execute one step, validate, repeat

### TDD Cycle

```
🔴 RED    →  Write failing tests
🟢 GREEN  →  Minimal implementation
🔵 BLUE   →  Refactor with green tests
```

### Boomerang Protocol

Every completed task returns structured data:

```json
{
  "type": "task-completed",
  "task_id": "auth-impl-001",
  "from": "green-phase",
  "status": "success",
  "files_changed": ["src/auth/jwt.ts"],
  "tests_run": ["npm test -- auth"],
  "summary": "Implemented JWT validation"
}
```

---

## IDE Support

Templates for every major AI-assisted IDE:

| IDE | Template Location |
|-----|-------------------|
| Roo Code | `templates/custom_modes.yaml` |
| Claude Code | `templates/claude-code/CLAUDE.md` |
| Cursor | `templates/cursor/rules/_global.mdc` |
| Copilot | `templates/copilot/copilot-instructions.md` |

---

## File Structure

| Path | Description |
|------|-------------|
| [`templates/custom_modes.yaml`](templates/custom_modes.yaml) | Mode definitions (13 modes with layer mappings) |
| [`templates/custom-instructions-for-all-modes.md`](templates/custom-instructions-for-all-modes.md) | Global contracts (paste into IDE) |
| [`templates/universal/AGENTS.md`](templates/universal/AGENTS.md) | Universal agent contract (OODA + TDD + Scope) |
| **Tool Integration** | |
| [`templates/tools/ooda-mcp.md`](templates/tools/ooda-mcp.md) | OODA MCP integration (Somatic layer) |
| [`templates/tools/synch-mcp.md`](templates/tools/synch-mcp.md) | Synch MCP integration (Autonomic layer) |
| [`templates/tools/index-foundry-mcp.md`](templates/tools/index-foundry-mcp.md) | Index Foundry integration (Autonomic layer) |
| [`templates/tools/trace-mcp.md`](templates/tools/trace-mcp.md) | Trace MCP integration (Reflex layer) |

---

## Philosophy

### Why a Nervous System?

LLMs are stateless text predictors. They have:
- ✅ High intelligence
- ❌ No agency
- ❌ No persistent memory
- ❌ No sensory-motor integration

A nervous system provides:
- **Closed loops** of action and sensation
- **Reflexes** that reject bad inputs immediately
- **Autonomic processes** that maintain state
- **Central coordination** of specialized capabilities

### From Chatbot to Organism

```
Chatbot:    User → LLM → Response
                   ↑
                   └── (no feedback)

Organism:   Environment → Senses → Brain → Motor → Environment
                 ↑                              ↓
                 └──────── Feedback ────────────┘
```

---

## Support

- ☕ [Buy Me a Coffee](https://buymeacoffee.com/Mnehmos)
- 🐛 [Report Issues](https://github.com/Mnehmos/mnehmos.multi-agent.framework/issues)
- 💬 [Discussions](https://github.com/Mnehmos/mnehmos.multi-agent.framework/discussions)

---

## License

MIT
