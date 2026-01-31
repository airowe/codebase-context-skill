# Example CLAUDE.md with Codebase Context Integration

This shows the recommended way to integrate codebase-context into your project's CLAUDE.md.

---

# My Project

One-sentence project description here.

## Monorepo

pnpm workspaces + Turborepo. Packages:

- `apps/web` — Next.js frontend
- `apps/api` — Express API server

See each package's `CLAUDE.md` for package-specific guidance.
For full project structure and workflows, see `.claude/codebase-context.md`.

## Commands

```bash
pnpm dev        # Start dev servers
pnpm typecheck  # Typecheck all packages
pnpm lint       # Lint all packages
pnpm test       # Run tests
```

## Rules

- Never commit without explicit approval
- Use Conventional Commits format

## Domain Concepts

| Term | Meaning |
|------|---------|
| Workspace | A team's shared environment |
| Member | A user within a workspace |
