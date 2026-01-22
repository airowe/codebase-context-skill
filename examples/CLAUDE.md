# Example CLAUDE.md with Codebase Context Integration

This shows how to integrate the codebase-context skill into your project's CLAUDE.md.

---

## CRITICAL: Read Codebase Context First

**BEFORE using Glob, Grep, or exploring the codebase, you MUST:**

1. Read `.claude/codebase-context.md` - contains pre-built project context
2. Run `.claude/check-context-freshness.sh` - verify context is current

**DO NOT** use file search tools (Glob, Grep, Task with Explore agent) until you have read the context file. The context file contains:
- Complete directory structure with file purposes
- Key files organized by feature
- Patterns, conventions, and code style
- Database schema and domain concepts
- Quick commands and common gotchas

If the freshness check returns "STALE", regenerate context before proceeding:
```
/skill codebase-context
```

This saves tokens and ensures accurate understanding of the codebase.

---

## Project Guidelines

[Your normal CLAUDE.md content goes here...]

### Code Style
- Use TypeScript strict mode
- Prefer functional components
- etc.

### Commands
```bash
pnpm dev      # Start dev server
pnpm test     # Run tests
pnpm build    # Production build
```
