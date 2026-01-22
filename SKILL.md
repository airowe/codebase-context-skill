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
- When staleness check indicates context is outdated

---

## Staleness Detection

Before reading codebase-context.md, check if it's stale by running:

```bash
# Check if context needs regeneration
.claude/check-context-freshness.sh
```

If the script exits non-zero or prints "STALE", regenerate the context.

### How Staleness Detection Works

When generating context, also create `.claude/codebase-context.snapshot` containing:
- Directory tree hash (top 3 levels)
- Key config file checksums (package.json, tsconfig.json, etc.)
- Generation timestamp

The freshness check compares current state against the snapshot.

### Generate the Freshness Check Script

After generating codebase-context.md, create `.claude/check-context-freshness.sh`:

```bash
#!/bin/bash
# Check if codebase-context.md needs regeneration

SNAPSHOT_FILE=".claude/codebase-context.snapshot"
CONTEXT_FILE=".claude/codebase-context.md"

# If no context file exists, it's stale
if [ ! -f "$CONTEXT_FILE" ]; then
    echo "STALE: No context file found"
    exit 1
fi

# If no snapshot exists, assume stale
if [ ! -f "$SNAPSHOT_FILE" ]; then
    echo "STALE: No snapshot file found"
    exit 1
fi

# Get current directory structure hash (top 3 levels, dirs only)
CURRENT_TREE=$(find . -maxdepth 3 -type d -not -path '*/\.*' -not -path './node_modules*' -not -path './dist*' -not -path './build*' -not -path './.next*' 2>/dev/null | sort | md5sum | cut -d' ' -f1)

# Get stored tree hash
STORED_TREE=$(grep "^tree:" "$SNAPSHOT_FILE" 2>/dev/null | cut -d' ' -f2)

if [ "$CURRENT_TREE" != "$STORED_TREE" ]; then
    echo "STALE: Directory structure changed"
    exit 1
fi

# Check key config files
for CONFIG in package.json tsconfig.json pyproject.toml Cargo.toml go.mod; do
    if [ -f "$CONFIG" ]; then
        CURRENT_HASH=$(md5sum "$CONFIG" 2>/dev/null | cut -d' ' -f1)
        STORED_HASH=$(grep "^$CONFIG:" "$SNAPSHOT_FILE" 2>/dev/null | cut -d' ' -f2)
        if [ "$CURRENT_HASH" != "$STORED_HASH" ]; then
            echo "STALE: $CONFIG changed"
            exit 1
        fi
    fi
done

# Check age (warn if older than 7 days)
GENERATED=$(grep "^generated:" "$SNAPSHOT_FILE" 2>/dev/null | cut -d' ' -f2)
if [ -n "$GENERATED" ]; then
    NOW=$(date +%s)
    AGE=$((NOW - GENERATED))
    DAYS=$((AGE / 86400))
    if [ $DAYS -gt 7 ]; then
        echo "STALE: Context is $DAYS days old (recommend regenerating weekly)"
        exit 1
    fi
fi

echo "FRESH: Context is up to date"
exit 0
```

### Generate the Snapshot

After generating codebase-context.md, create `.claude/codebase-context.snapshot`:

```bash
#!/bin/bash
# Generate snapshot for freshness detection

SNAPSHOT_FILE=".claude/codebase-context.snapshot"

# Directory tree hash
TREE_HASH=$(find . -maxdepth 3 -type d -not -path '*/\.*' -not -path './node_modules*' -not -path './dist*' -not -path './build*' -not -path './.next*' 2>/dev/null | sort | md5sum | cut -d' ' -f1)

echo "tree: $TREE_HASH" > "$SNAPSHOT_FILE"
echo "generated: $(date +%s)" >> "$SNAPSHOT_FILE"

# Hash key config files
for CONFIG in package.json tsconfig.json pyproject.toml Cargo.toml go.mod; do
    if [ -f "$CONFIG" ]; then
        HASH=$(md5sum "$CONFIG" | cut -d' ' -f1)
        echo "$CONFIG: $HASH" >> "$SNAPSHOT_FILE"
    fi
done

echo "Snapshot saved to $SNAPSHOT_FILE"
```

---

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

### Step 3: Save Files

Save to the project:
1. `.claude/codebase-context.md` - The context document
2. `.claude/codebase-context.snapshot` - Freshness snapshot
3. `.claude/check-context-freshness.sh` - Freshness check script (make executable)

```bash
chmod +x .claude/check-context-freshness.sh
```

### Step 4: Update CLAUDE.md

Add this block at the **TOP** of the project's `CLAUDE.md` (before any other instructions):

```markdown
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
```

**Why this instruction works:**
- Placed at the TOP of CLAUDE.md so it's read first
- Uses "CRITICAL" and "MUST" for priority
- Explicitly lists the tools to avoid until context is read
- Explains the benefit (saves tokens, accurate understanding)

---

## Quick Regeneration

When context is stale, regenerate with:

```
Generate fresh codebase context for this project
```

Or use the skill directly:
```
/skill codebase-context
```

---

## Best Practices

1. **Check freshness first** - Run the check script before trusting the context
2. **Keep it updated** - Regenerate after major changes
3. **Be specific** - Include actual file paths, not generic descriptions
4. **Prioritize** - Put most important info first
5. **Be concise** - Agents have limited context; don't pad with fluff
6. **Include gotchas** - Document non-obvious behaviors that waste tokens
7. **Commit the files** - Version control the context and snapshot

## Example Output Structure

```
.claude/
├── codebase-context.md           # Generated context file
├── codebase-context.snapshot     # Freshness snapshot
└── check-context-freshness.sh    # Freshness check script

CLAUDE.md                          # References codebase-context.md
```

## Maintenance

The staleness check will detect when regeneration is needed:
- Directory structure changes (new folders, reorganization)
- Config file changes (package.json, tsconfig.json, etc.)
- Age > 7 days

Manual regeneration triggers:
- Adding new major features
- Changing tech stack components
- Modifying conventions or patterns
- Noticing agents repeatedly exploring the same areas
