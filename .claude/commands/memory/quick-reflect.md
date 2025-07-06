---
description: Extract critical session insights for memory optimization
allowed-tools:
  - Read
  - Edit
  - TodoWrite
---

# Quick Session Insight Capture

## Context
Memory files are optimized for token efficiency. Only high-frequency, actionable insights should be added.

## Your Task

**CRITICAL**: Do NOT add dated session learnings to memory files. Instead:

### 1. Identify Actionable Insights
- **Command/workflow** that was forgotten and caused delay
- **Critical warning** that prevents major issues  
- **Pattern** that will be used frequently

### 2. Update Strategy
- **High-frequency issues** → Add to relevant CLAUDE.md
- **Detailed patterns** → Update @doc/ files
- **One-time learnings** → Document in session notes only

### 3. Memory File Updates (if applicable)
```bash
# Root CLAUDE.md: Only monorepo-critical items
# API CLAUDE.md: Only NestJS/Prisma patterns used frequently  
# Web CLAUDE.md: Only React/testing patterns used frequently
```

### 4. Anti-Patterns to Avoid
- ❌ Adding dated "Session Learnings" sections
- ❌ Including temporary or one-time issues
- ❌ Expanding memory files beyond size limits
- ❌ Duplicating content already in @doc/ files

### 5. Validation
- Will this insight be needed in >50% of sessions?
- Is it more essential than existing content?
- Does it fit the optimized memory structure?

**Remember**: Memory files are for immediate actionable guidance, not comprehensive session logs.