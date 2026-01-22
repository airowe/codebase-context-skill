---
name: codebase-context
description: Generates a codebase-context.md file that provides pre-built context for AI agents. This skill analyzes your project structure, patterns, and conventions to create a comprehensive context document that reduces token usage and improves agent effectiveness. Use when setting up a new project for AI-assisted development or when the codebase has significantly changed.
---

# Codebase Context Generator

## Purpose

Generate a `.claude/codebase-context.md` file that provides pre-built context for AI agents working on your codebase. This reduces exploration tokens and helps agents understand your project immediately.

## When to Use

- Setting up a new project for AI-assisted development
- After significant architectural changes
- When onboarding new team members who use AI tools
- When agents are spending too many tokens exploring the codebase

## Generation Process

### Step 1: Analyze Project Structure

Explore the codebase to understand:

1. **Project type and stack** - Framework, language, package manager
2. **Directory structure** - Key folders and their purposes
3. **Key files by feature** - Important files organized by domain
4. **Patterns & conventions** - Naming, code style, testing approach

### Step 2: Document Key Information

Create sections for:

```markdown
# Codebase Context

> **INSTRUCTION FOR AGENTS:** Read this file FIRST before exploring the codebase.

## Project Overview
- Name, description, type
- Tech stack summary
- Package manager and monorepo status

## Directory Structure
- Tree view of important directories
- Purpose of each major folder

## Key Files by Feature
- Group important files by domain/feature
- Include file paths and brief descriptions

## Patterns & Conventions
- Naming conventions (files, functions, types, components)
- Code style rules
- Testing approach and location
- API design patterns

## Tech Stack Table
| Layer | Technology |
|-------|------------|
| Framework | ... |
| Language | ... |
| ... | ... |

## Database Schema (if applicable)
- Key tables and their purposes
- Important relationships

## Important Rules
- Critical constraints agents must follow
- Pre-commit/CI requirements

## Quick Commands
- Development commands
- Quality gates
- Build/deploy commands

## Environment Variables
- Required env vars (without values)

## Common Gotchas
- Non-obvious behaviors
- Platform-specific issues
- Unusual configurations

## Domain Concepts
- Key terminology and definitions
- Business logic concepts

## Key Workflows
- Important user/data flows
- Integration patterns
```

### Step 3: Place the File

Save to `.claude/codebase-context.md` in the project root.

### Step 4: Reference in CLAUDE.md

Add to the project's `CLAUDE.md`:

```markdown
## IMPORTANT: Read Codebase Context First

**Before exploring the codebase, ALWAYS read `.claude/codebase-context.md` first.**

This file contains pre-built context about:
- Directory structure and key files
- Patterns and conventions
- Database schema
- Common workflows
- Quick commands
```

## Best Practices

1. **Keep it updated** - Regenerate after major changes
2. **Be specific** - Include actual file paths, not generic descriptions
3. **Prioritize** - Put most important info first
4. **Be concise** - Agents have limited context; don't pad with fluff
5. **Include gotchas** - Document non-obvious behaviors that waste tokens

## Example Output Structure

```
.claude/
├── codebase-context.md    # Generated context file
└── ...

CLAUDE.md                   # References codebase-context.md
```

## Maintenance

Re-run this skill when:
- Adding new major features or directories
- Changing tech stack components
- Modifying conventions or patterns
- Noticing agents repeatedly exploring the same areas
