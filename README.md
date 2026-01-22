# Codebase Context Skill

A Claude Code skill that generates comprehensive context documents for AI agents working on your codebase.

## What It Does

This skill creates a `.claude/codebase-context.md` file that provides pre-built context about your project. Instead of agents spending tokens exploring your codebase every session, they can read this context file first and immediately understand:

- Project structure and architecture
- Key files organized by feature
- Naming conventions and code style
- Database schema
- Common commands
- Domain concepts and workflows
- Gotchas and non-obvious behaviors

## Installation

### Option 1: Clone to skills directory

```bash
git clone https://github.com/anthropics/codebase-context-skill ~/.claude/skills/codebase-context
```

### Option 2: Symlink from your preferred location

```bash
git clone https://github.com/anthropics/codebase-context-skill ~/path/to/skills/codebase-context
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

## Example Output

See [examples/codebase-context.md](examples/codebase-context.md) for a sample generated context file.

## Why Use This?

**Before:** Every Claude Code session starts with the agent exploring your codebase, using tokens to understand structure, patterns, and conventions.

**After:** Agents read the pre-built context file and immediately understand your project, saving tokens and providing more accurate assistance.

## Best Practices

1. **Regenerate after major changes** - Keep the context file current
2. **Reference in CLAUDE.md** - Tell agents to read it first
3. **Be specific** - Include actual file paths, not generic descriptions
4. **Include gotchas** - Document non-obvious behaviors

## Contributing

PRs welcome! Please follow the existing format and include examples.

## License

MIT
