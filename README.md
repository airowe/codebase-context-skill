# Codebase Context Skill

A Claude Code skill that generates lean context documents for AI agents, with automatic staleness detection.

## What It Does

Creates pre-built context files so agents understand your codebase without blind exploration:

| File | Purpose | Format |
|------|---------|--------|
| `codebase-context.md` | Human-readable project overview (80-150 lines) | Markdown |
| `code-index.json` | Fast lookups for concepts, exports, types | JSON |
| `deps.mermaid` | Dependency graph for tracing imports | Mermaid |

## Installation

```bash
git clone https://github.com/airowe/codebase-context-skill ~/.claude/skills/codebase-context
```

## Usage

### Generate context files

```bash
# Generate all machine-readable files (code-index.json, deps.mermaid, deps.json)
~/.claude/skills/codebase-context/scripts/generate-all.sh .

# Generate human-readable context via Claude
/codebase-context
```

### Integrate with CLAUDE.md

Add one line to your project's `CLAUDE.md`:

```markdown
For full project structure and workflows, see `.claude/codebase-context.md`.
```

That's it. The agent reads the context file when it needs to explore, skips it for targeted tasks.

**Don't** add a "CRITICAL: Read First" block — it forces the context to load on every request, wasting tokens on targeted tasks. See [SKILL.md](SKILL.md) for details on why.

## Generated Files

```
.claude/
├── codebase-context.md           # Human-readable context (80-150 lines)
├── code-index.json               # Machine-optimized lookups
├── deps.mermaid                  # Dependency graph (Mermaid)
├── codebase-context.snapshot     # Freshness snapshot
└── check-context-freshness.sh    # Freshness check script
```

## Staleness Detection

```bash
.claude/check-context-freshness.sh
```

Detects:
- Directory structure changes
- Config file changes (package.json, tsconfig.json, etc.)
- Age > 7 days

## Best Practices

1. **Progressive disclosure** — Reference context from CLAUDE.md, don't mandate preloading
2. **Keep it lean** — Target 80-150 lines; over 200 means you should split content
3. **Concepts over file paths** — Domain vocabulary is stable; file paths change constantly
4. **Monorepo structure** — Use package-level `CLAUDE.md` for package-specific guidance
5. **Commit the files** — Version control context, snapshot, and check script

### What to include in context files

- One-line project description
- Directory tree (top 2-3 levels)
- Domain concepts/vocabulary
- Key workflows (1 line each)
- Patterns and conventions
- Gotchas

### What to leave out

- Environment variable examples (sensitive, discoverable from `.env.example`)
- Code snippets / API usage examples (go stale, agent can read actual code)
- Full file listings per screen/feature (too granular, changes frequently)
- Deployment / App Store details (rarely relevant to coding tasks)
- Marketing content

## Scripts

| Script | Purpose | Output |
|--------|---------|--------|
| `generate-all.sh` | Run all generators | All files |
| `generate-code-index.sh` | Extract concepts, exports, types, routes | `code-index.json` |
| `generate-deps.sh` | Build dependency graph | `deps.mermaid`, `deps.json` |

Supported languages: TypeScript/JavaScript, Python, Go (partial).

For best TypeScript/JavaScript results, install [madge](https://github.com/pahen/madge):
```bash
npm install -g madge
```

## Example Output

- [examples/codebase-context.md](examples/codebase-context.md)
- [examples/code-index.json](examples/code-index.json)
- [examples/deps.mermaid](examples/deps.mermaid)
- [examples/CLAUDE.md](examples/CLAUDE.md)

## License

MIT
