# AI Project Template

A reusable AI-first project template that provides a full AI development workflow for **GitHub Copilot** (VS Code) and **Claude Code** (CLI). Drop this into any new project to get an intelligent, self-learning AI assistant with persistent knowledge base, automated fact extraction, and comprehensive agent/skill/rule infrastructure.

## What's Included

### 🧠 Knowledge Base System (`knowledge_base/`)
- **Qdrant vector database** (embedded, no server required) for semantic search
- **Two collections**: session memory (curated facts) + document chunks (auto-indexed files)
- **Auto-deduplication** at 0.92 cosine similarity
- **Auto-extraction hooks** that learn from every conversation
- **Context injection** that surfaces relevant knowledge on every prompt
- **CLI interface** via `scripts/kb.py`

### 🤖 AI Agents (`.github/agents/`)
- `github-agent.md` — Full-stack development agent
- `reviewer-agent.md` — Code review agent
- `tester-agent.md` — Testing and QA agent
- `screenshot-agent.md` — Visual documentation agent
- `template-agent.md` — Base template for custom agents
- `ultimate-agent.md` — All-purpose development agent

### 📏 Rules & Instructions
- `.claude/_rules/` — Python, TypeScript, and development process rules
- `.github/_instructions/` — Language-specific coding conventions
- `.github/copilot-instructions.md` — Core AI behavior rules

### 🔧 Hooks (`.github/hooks/`)
- **UserPromptSubmit** — Searches KB and injects relevant context before every response
- **PreCompact / SubagentStop / SessionEnd** — Extracts and stores knowledge from conversations

### 🛠 MCP Servers (`.mcp.json`)
- Chrome DevTools — Browser automation and testing
- Desktop Commander — System interaction
- Playwright — Browser testing
- Time — Timezone-aware operations

### 📁 Skills & Commands
- Knowledge base management skill
- Audit instructions skill
- Extensible command system

## Quick Start

### 1. Copy to Your Project

```bash
# Option A: Clone and copy
git clone https://github.com/VillaKeth/ai-project-template.git
cp -r ai-project-template/{.claude,.github,.mcp.json,.gitignore,knowledge_base,scripts,TODO.md} your-project/

# Option B: Use as a template on GitHub
# Click "Use this template" on the GitHub repo page
```

### 2. Install Python Dependencies

```bash
# Create a virtual environment
python -m venv venv

# Activate it
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install KB dependencies
pip install qdrant-client fastembed
```

### 3. Customize for Your Project

1. **Edit `.claude/_CLAUDE.md`** — Replace the template content with your project's overview, tech stack, and architecture.

2. **(Optional) Create `kb_sources.json`** in your project root to tell the KB which files to index:
   ```json
   [
     {"dir": "src", "pattern": "**/*.py"},
     {"dir": "docs", "pattern": "**/*.md"},
     {"dir": "frontend/src", "pattern": "**/*.{ts,tsx}"},
     {"dir": ".", "pattern": "README.md"}
   ]
   ```
   If you skip this, the KB auto-detects common directories (src, lib, app, frontend, backend, docs).

3. **Run initial document ingestion:**
   ```bash
   python scripts/kb.py ingest
   python scripts/kb.py status
   ```

4. **Add project-specific agents** to `.github/agents/` (copy `template-agent.md` as a starting point).

5. **Add project-specific rules** to `.claude/_rules/` for your framework/language conventions.

### 4. Configure Hooks (Claude Code)

The `.claude/settings.json` file configures hooks that auto-run on every prompt. The default uses `python` directly. If your project uses a virtual environment, update the commands:

```json
{
  "hooks": {
    "UserPromptSubmit": [{
      "type": "command",
      "command": "venv/Scripts/python .github/hooks/read_knowledge.py"
    }]
  }
}
```

Replace `venv/Scripts/python` with your venv path (`venv/bin/python` on macOS/Linux).

## Directory Structure

```
your-project/
├── .claude/                     # Claude Code configuration
│   ├── _CLAUDE.md               # Project overview (edit this!)
│   ├── settings.json            # Permissions & hooks
│   ├── ralph-loop.local.md      # Auto-documentation loop config
│   ├── _rules/                  # Coding rules
│   │   ├── python.md
│   │   ├── typescript.md
│   │   └── development-process.md
│   ├── commands/                # Custom slash commands (add yours)
│   ├── skills/                  # Claude skills
│   │   └── audit-instructions/
│   └── plans/                   # Session plans (auto-generated)
├── .github/                     # GitHub Copilot configuration
│   ├── copilot-instructions.md  # Core AI behavior rules
│   ├── agents/                  # AI agents for VS Code
│   ├── hooks/                   # Knowledge extraction/injection hooks
│   ├── skills/                  # Skills (knowledge-base)
│   ├── _instructions/           # Language-specific instructions
│   ├── prompts/                 # Saved prompts
│   └── issues/                  # Issue templates
├── knowledge_base/              # Vector knowledge base module
│   ├── config.py                # Configuration (repo-agnostic)
│   ├── store.py                 # CRUD operations
│   ├── extractor.py             # Transcript fact extraction
│   ├── categories.py            # Knowledge categorization
│   ├── chunker.py               # Document chunking
│   ├── ingestion.py             # File indexing pipeline
│   ├── dedup.py                 # Deduplication logic
│   ├── client.py                # Qdrant client wrapper
│   └── backends.py              # Protocol interfaces
├── scripts/
│   ├── kb.py                    # Knowledge base CLI wrapper
│   └── venv_helper.py           # Cross-platform venv helper
├── .mcp.json                    # MCP server configuration
├── .gitignore                   # Git ignore rules
├── TODO.md                      # Project task tracking
└── artifacts/                   # AI workspace (gitignored)
```

## Knowledge Base Usage

```bash
# Search
python scripts/kb.py search "your query"
python scripts/kb.py search "topic" --unified          # Search both KB + docs

# Add knowledge
python scripts/kb.py add --text "Important fact" --category lesson_learned

# Index project files
python scripts/kb.py ingest
python scripts/kb.py ingest --changed                   # Incremental

# Dashboard
python scripts/kb.py status

# Categories: architecture_decision, bug_fix, convention, lesson_learned,
#             configuration, preference, api_knowledge, troubleshooting
```

## Requirements

- **Python 3.10+** with `qdrant-client` and `fastembed` packages
- **Node.js 18+** (for MCP servers via `npx`)
- **GitHub Copilot** (VS Code) and/or **Claude Code** (CLI)

## License

MIT — Use freely in any project.
