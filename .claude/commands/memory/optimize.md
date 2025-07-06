---
description: Comprehensive memory optimization and token efficiency analysis
allowed-tools:
  - Read
  - Edit
  - MultiEdit
  - Bash
---

# Memory Optimization

## Current Memory Structure
Root (26), API (83), Web (58) = 167 lines | Target: <200 | @doc/ for details

## Task

### Step 1: Analysis
```bash
# Check current memory file sizes
wc -l CLAUDE.md api/CLAUDE.md web-back-office/CLAUDE.md

# Identify optimization opportunities
```

### Step 2: Content Audit
**Extract to @doc/ files:**
- Code examples >5 lines
- Detailed explanations  
- Comprehensive guides
- One-time setup instructions

**Keep in memory files:**
- Commands used >50% of sessions
- Critical warnings preventing issues
- Package manager enforcement
- Port/directory checks

### Step 3: Optimization Process
1. **Content Audit**: Identify redundant/verbose content across files
2. **Extraction**: Move detailed patterns to @doc/ files
3. **Compression**: Merge similar rules, compress command listings
4. **Validation**: Verify essential workflows preserved

### Step 4: Quality Gates
✅ Essential workflows preserved (pwd, port checks, package managers)
✅ Size targets met (Root <30, API <100, Web <80)  
✅ @doc/ references functional
✅ Critical warnings maintained
✅ No redundant information

## Memory File Targets
```
Root CLAUDE.md: <30 lines (monorepo essentials)
API CLAUDE.md: <100 lines (NestJS quick patterns)  
Web CLAUDE.md: <80 lines (React essentials)
```

## Anti-Patterns to Remove
❌ Dated session content ("Session Learnings (2025-XX-XX)")
❌ Meta-maintenance instructions  
❌ Verbose code examples in memory files
❌ Redundant tech stack info across files
❌ Temporary workflow notes

## Documentation Structure
- **@doc/api-patterns.md**: Complete NestJS service/controller/DTO patterns
- **@doc/nestjs-testing.md**: Comprehensive testing strategies
- **@doc/react-patterns.md**: React components, hooks, error handling
- **@doc/development-patterns.md**: Cross-project workflows
- **@doc/git-workflow.md**: Submodule management

## Validation Checklist
- [x] Directory check: `pwd` command guidance
- [x] Port checks: `lsof -i :3000` and `lsof -i :5173`
- [x] Package manager enforcement: bun vs pnpm
- [x] Submodule workflow: absolute paths, commit order
- [x] Navigation commands: `/project:workdir @api|@web-back-office|@root`

**Focus**: Maximize actionable guidance per token while maintaining full functionality.