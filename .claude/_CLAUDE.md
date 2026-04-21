# Project Name

## Project Overview

<!-- Replace this with your project description -->
Describe your project here. What does it do? What problem does it solve?

## Tech Stack

<!-- List your technologies -->
- **Frontend**: 
- **Backend**: 
- **Database**: 
- **Other**: 

## Common Commands

```bash
# Add your common commands here
# npm start        # Start the application
# npm test         # Run tests
# npm run build    # Build for production
```

## Architecture

<!-- Describe your project's architecture -->
Describe the high-level architecture, services, and how they communicate.

## Directory Structure

```
├── .claude/          # Claude Code AI configuration
├── .github/          # GitHub Copilot AI configuration
├── knowledge_base/   # Vector knowledge base (Qdrant)
├── scripts/          # Utility scripts (kb.py, venv_helper.py)
├── artifacts/        # Temporary AI workspace files
├── docs/             # Project documentation
└── ...               # Your project files
```

## AI Agent Notes

- Use the knowledge base (`python scripts/kb.py`) to search for project-specific knowledge before investigating files.
- The knowledge base auto-extracts facts from conversations and injects relevant context on each prompt.
- Run `python scripts/kb.py status` to see KB health.
- Run `python scripts/kb.py ingest` to index project files into the document collection.
