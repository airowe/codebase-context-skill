# Codebase Context Skill

A Claude Code skill that generates comprehensive context documents for AI agents working on your codebase, with automatic staleness detection.

## What It Does

This skill creates a `.claude/codebase-context.md` file that provides pre-built context about your project. Instead of agents spending tokens exploring your codebase every session, they can read this context file first and immediately understand:

- Project structure and architecture
- Key files organized by feature
- Naming conventions and code style
- Database schema
- Common commands
- Domain concepts and workflows
- Gotchas and non-obvious behaviors

## Staleness Detection

The skill includes automatic freshness checking:

```bash
# Check if context needs regeneration
.claude/check-context-freshness.sh
```

The check detects:
- **Directory structure changes** - New folders, reorganization
- **Config file changes** - package.json, tsconfig.json, etc.
- **Age > 7 days** - Recommends weekly regeneration

## Installation

### Option 1: Clone to skills directory

```bash
git clone https://github.com/airowe/codebase-context-skill ~/.claude/skills/codebase-context
```

### Option 2: Symlink from your preferred location

```bash
git clone https://github.com/airowe/codebase-context-skill ~/path/to/skills/codebase-context
ln -s ~/path/to/skills/codebase-context ~/.claude/skills/codebase-context
```

## Usage

The skill triggers automatically when you ask Claude to:
- "Generate codebase context"
- "Create a context file for this project"
- "Document this codebase for AI agents"

Or reference it directly:
```
/skill codebase-context
```

## Generated Files

After running the skill, your project will have:

```
.claude/
├── codebase-context.md           # The context document
├── codebase-context.snapshot     # Freshness snapshot (hashes + timestamp)
└── check-context-freshness.sh    # Freshness check script
```

## Example Output

See [examples/codebase-context.md](examples/codebase-context.md) for a sample generated context file.

## Why Use This?

**Before:** Every Claude Code session starts with the agent exploring your codebase, using tokens to understand structure, patterns, and conventions.

**After:** Agents read the pre-built context file and immediately understand your project, saving tokens and providing more accurate assistance.

## Best Practices

1. **Check freshness first** - Run the check script before trusting the context
2. **Regenerate after major changes** - The staleness check will remind you
3. **Reference in CLAUDE.md** - Tell agents to read it first
4. **Be specific** - Include actual file paths, not generic descriptions
5. **Commit all files** - Version control the context, snapshot, and check script

## Companion Tools

The codebase-context file provides static, high-level understanding. For deeper dynamic exploration, consider:

### [grepai](https://github.com/yoanbernabeu/grepai) - Semantic Code Search

Search code by meaning, not just text patterns. Query "user authentication flow" instead of grepping for function names.

```bash
# Install & setup
curl -sSL https://raw.githubusercontent.com/yoanbernabeu/grepai/main/install.sh | sh
cd your-project && grepai init && grepai watch

# Search by meaning
grepai search "error handling in API routes"

# Trace call graphs
grepai trace callers myFunction
```

- 100% local (uses Ollama)
- MCP server for Claude Code integration
- Real-time indexing

### Recommended Workflow

| Step | Tool | Purpose |
|------|------|---------|
| 1 | codebase-context | Understand project structure |
| 2 | grepai | Find code by semantic meaning |
| 3 | Glob/Grep | Exact pattern matching |

## Contributing

PRs welcome! Please follow the existing format and include examples.

## License

MIT
