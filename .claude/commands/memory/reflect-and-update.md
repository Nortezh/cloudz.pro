---
description: Optimize memory files using systematic workflow analysis
allowed-tools:
  - Read
  - Edit
  - MultiEdit
  - TodoWrite
  - Bash
---

# Memory Optimization Workflow

## Context
- Memory structure: Root (26 lines), API (83 lines), Web (58 lines)
- Token efficiency target: <200 total lines
- Documentation: @doc/ files for detailed patterns

## Your Task

Analyze session patterns and optimize memory files for maximum efficiency.

### 1. Usage Pattern Analysis
```bash
# Check current memory file sizes
wc -l CLAUDE.md api/CLAUDE.md web-back-office/CLAUDE.md

# Identify optimization opportunities
```

### 2. Content Optimization Strategy

**Extract to @doc/ files:**
- Code examples >5 lines
- Detailed explanations
- Comprehensive guides
- One-time setup instructions

**Keep in memory files:**
- Commands used >50% of sessions
- Critical warnings that prevent issues
- Package manager enforcement
- Port/directory checks

### 3. Systematic Updates

**Step 1: Content Audit**
- Identify redundant content across files
- Find verbose sections that can be compressed
- Locate content that belongs in @doc/ files

**Step 2: Optimization**
- Compress command listings
- Merge similar rules
- Replace examples with references
- Remove temporary content

**Step 3: Validation**
- Verify essential workflows preserved
- Check cross-references work
- Confirm size targets met

### 4. Memory File Targets
```
Root CLAUDE.md: <30 lines (monorepo essentials)
API CLAUDE.md: <100 lines (NestJS quick patterns)
Web CLAUDE.md: <80 lines (React essentials)
```

### 5. Quality Gates
- ✅ All critical commands preserved
- ✅ Package manager warnings maintained  
- ✅ Cross-references to @doc/ functional
- ✅ Size limits respected
- ✅ No redundant information

### 6. Anti-Patterns to Remove
- ❌ Dated session content
- ❌ Meta-maintenance instructions
- ❌ Verbose code examples
- ❌ Redundant tech stack info
- ❌ Temporary workflow notes

**Focus**: Maximize actionable guidance per token while maintaining full functionality.