# Global User Preferences Template for Claude Code

This is a template for creating your personal Claude Code preferences.
Copy this file to `~/.claude/CLAUDE.md` and customize it with your preferences.

```bash
mkdir -p ~/.claude
cp CLAUDE_USER_TEMPLATE.md ~/.claude/CLAUDE.md
```

## Example User Preferences

```markdown
# CLAUDE.md - Personal Preferences

This file contains my personal preferences for Claude Code across all projects.

## Communication Style
- Be concise and direct
- Skip explanations unless asked
- Focus on implementation

## Development Preferences
- Prefer functional programming patterns
- Use early returns over nested conditionals
- Extract complex logic into well-named functions

## Testing Preferences
- Write tests for edge cases
- Use descriptive test names
- Group related tests

## Code Review Focus
- Security vulnerabilities
- Performance bottlenecks
- Code maintainability

## Personal Tools
- Editor: VS Code with vim mode
- Terminal: iTerm2 with oh-my-zsh
- Git: Use interactive rebase for clean history

## Shortcuts I Use
- `cmd+k` - Clear terminal
- `cmd+shift+p` - Command palette
- `gst` - git status (oh-my-zsh alias)
```

## What to Include in Your Personal CLAUDE.md

1. **Communication preferences** - How you like Claude to respond
2. **Coding style preferences** - Personal patterns you prefer
3. **Tool configurations** - Your specific setup
4. **Common workflows** - How you typically work
5. **Project-specific overrides** - Special handling for certain projects

## What NOT to Include

1. **Secrets or credentials** - Never store sensitive data
2. **Project-specific code** - Keep that in project CLAUDE.md files
3. **Team standards** - Those belong in project files
4. **Temporary notes** - Clean up regularly

## Tips

- Keep it concise - this loads with every Claude Code session
- Update regularly as your preferences evolve
- Use markdown formatting for clarity
- Test changes to ensure they improve Claude's responses