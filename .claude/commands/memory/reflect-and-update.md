---
description: Session reflection and strategic memory updates
allowed-tools:
  - Read
  - Edit
  - MultiEdit
  - TodoWrite
---

# Reflect and Update Memory

## Context
Optimized memory: Root (26), API (83), Web (58) = 167 lines | Token-efficient structure

## Task

Extract critical learnings from this session and strategically update memory files.

### Step 1: Session Analysis
**Identify high-value insights:**
- Commands/workflows forgotten that caused significant delays
- Critical warnings that prevent major issues  
- Patterns discovered that will be used frequently (>50% of sessions)
- Workflow improvements with measurable impact

### Step 2: Impact Assessment
**For each potential insight, evaluate:**
- **Frequency**: Will this be needed in >50% of future sessions?
- **Criticality**: Does this prevent significant delays/errors?
- **Priority**: Is this more important than existing memory content?
- **Scope**: Root (monorepo), API (NestJS), or Web (React) specific?

### Step 3: Strategic Updates

**Root CLAUDE.md Updates:**
- Monorepo navigation improvements
- Cross-project workflow enhancements  
- Critical submodule handling patterns
- Package manager enforcement warnings

**Project-Specific Updates:**
- **API CLAUDE.md**: NestJS/Prisma patterns, testing workflows
- **Web CLAUDE.md**: React/TanStack Query patterns, TDD workflows

**Documentation Updates:**
- **@doc/ files**: Detailed patterns, comprehensive guides
- Move verbose content OUT of memory files

### Step 4: Update Guidelines

**Add to memory files ONLY if:**
✅ Used frequently (>50% of sessions)
✅ Prevents significant issues/delays
✅ More critical than existing content
✅ Fits within size limits (Root <30, API <100, Web <80)

**Move to @doc/ files:**
- Detailed explanations
- Comprehensive examples  
- Reference material
- One-time setup guides

### Step 5: Anti-Patterns to Avoid
❌ Adding dated "Session Learnings" sections
❌ Including temporary or one-time issues  
❌ Expanding memory files beyond size limits
❌ Duplicating content already in @doc/ files
❌ Meta-maintenance instructions

### Step 6: Validation
After updates, verify:
- Size limits maintained
- Essential workflows preserved
- @doc/ references functional
- No redundant information
- Critical warnings intact

## Update Strategy
1. **High-frequency command/warning** → Memory file
2. **Detailed pattern/example** → @doc/ file  
3. **One-time issue/learning** → Session notes only
4. **Process improvement** → @doc/development-patterns.md

**Remember**: Memory files are for immediate actionable guidance, not comprehensive session logs. Focus on preventing the most common and critical issues while maintaining token efficiency.