---
description: Ultra-efficient memory optimization and session insight capture
allowed-tools:
  - Read
  - Edit
  - MultiEdit
  - Bash
---

# Memory Optimization + Session Insights

## Context
Current: Root (26), API (83), Web (58) = 167 lines | Target: <200 | @doc/ for details

## Task

**Step 1: Size Check**
```bash
wc -l CLAUDE.md api/CLAUDE.md web-back-office/CLAUDE.md
```

**Step 2: Critical Insights Only**
Identify patterns meeting ALL criteria:
- Used in >50% of sessions
- Prevents significant delays/errors  
- More critical than existing content

**Step 3: Optimization (if needed)**
- Extract verbose examples to @doc/
- Compress redundant command listings
- Merge similar rules
- Remove temporary content

**Step 4: Update Strategy**
- **High-frequency command/warning** → Memory file
- **Detailed pattern** → @doc/ file  
- **One-time issue** → Session notes only

## Anti-Patterns
❌ Dated session learnings  
❌ Verbose examples in memory  
❌ Exceeding size limits  
❌ Meta-maintenance content

## Quality Gates
✅ Essential workflows preserved  
✅ Size targets met (<30, <100, <80)  
✅ @doc/ references functional  
✅ Critical warnings maintained

**Focus**: Maximum actionable guidance per token.