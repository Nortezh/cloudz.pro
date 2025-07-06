---
description: Create optimized custom slash command from user purpose
---

Create a custom slash command based on your requirements.

## Current project structure:
!`find "$(git rev-parse --show-toplevel 2>/dev/null || pwd)/.claude/commands" -name "*.md" 2>/dev/null | xargs basename -s .md | sed 's/^/\/project:/' || echo "No existing commands"`

## Available command features:
- **Arguments**: Use `$ARGUMENTS` placeholder for user input
- **Dynamic content**: Use `!` prefix for bash commands that run when command loads
- **File references**: Use `@` prefix to include file contents
- **Frontmatter options**:
  - `description`: Brief command description
  - `allowed-tools`: Restrict tools (e.g., `Bash(git:*, npm:*)`)
  - `extended-thinking`: Enable for complex analysis

## Your task:
1. Ask the user what command they want to create
2. Determine the command name and purpose
3. Create an optimized `.md` file in `.claude/commands/`
4. Follow best practices:
   - Keep commands focused and concise
   - Use dynamic content where beneficial
   - Include clear usage instructions
   - Add appropriate frontmatter

Provide the command name and describe what it should do.