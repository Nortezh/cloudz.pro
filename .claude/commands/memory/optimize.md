---
description: Optimize memory files according to auto-update rules
allowed-tools:
  - Read
  - Edit
  - MultiEdit
  - Glob
---

# Memory File Optimization

## Context
- Memory optimization rules: Root (100 lines), API (200 lines), Web (80 lines), Portal (60 lines)
- Auto-update cycles: Root (10 sessions), Projects (5 sessions)
- Documentation location: @doc/ files for detailed content

## Your Task

Optimize all memory files according to the established auto-update rules and memory optimization guidelines.

### Memory File Targets

**Root CLAUDE.md (100-line limit)**
- Check current line count and optimize if approaching limit
- Remove instructions unused in last 10 sessions
- Consolidate similar rules into single statements
- Move detailed content to @doc/ files if needed
- Run validation tests: directory check, port check, commit format, external docs

**Project CLAUDE.md Files**
- **API** (200-line limit): Remove unused patterns older than 5 sessions
- **Web Back Office** (80-line limit): Keep essential commands only
- **Web Portal** (60-line limit): Optimize for future implementation

**Documentation Files (@doc/)**
- Consolidate moved content from memory files
- Remove outdated information
- Update cross-references

### Optimization Process
1. **Audit**: Count lines and identify unused content
2. **Consolidate**: Merge similar rules and remove duplicates
3. **Relocate**: Move detailed content to appropriate @doc/ files
4. **Validate**: Ensure critical functionality remains accessible
5. **Test**: Verify all references and examples work correctly

### Success Criteria
- All memory files within specified line limits
- No loss of critical functionality
- Improved clarity and conciseness
- Proper cross-referencing to detailed documentation
- Validated that essential workflows still function

## Requirements
- Maintain all critical functionality
- Preserve essential patterns and commands
- Update cross-references when moving content
- Test that moved content is still accessible