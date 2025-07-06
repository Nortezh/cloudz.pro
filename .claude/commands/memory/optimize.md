# Memory Optimization Command

## Current Memory Structure (Optimized)

### Root CLAUDE.md (26 lines) ✅
- **Target**: <30 lines, monorepo essentials only
- **Content**: Navigation, commits, development rules, ports, tech stack
- **Scope**: Cross-project guidelines and workflows

### API CLAUDE.md (83 lines) ✅  
- **Target**: <100 lines, API-specific guidance
- **Content**: Bun commands, NestJS patterns, error handling, quick setup
- **Scope**: Backend development with NestJS + Prisma

### Web Back Office CLAUDE.md (58 lines) ✅
- **Target**: <80 lines, frontend-specific guidance  
- **Content**: PNPM commands, React patterns, TanStack Query, TDD workflow
- **Scope**: React frontend development

## Documentation Structure

### Detailed Patterns (Moved to @doc/)
- **@doc/api-patterns.md**: Complete NestJS service/controller/DTO patterns
- **@doc/nestjs-testing.md**: Comprehensive testing strategies
- **@doc/react-patterns.md**: React components, hooks, error handling

### Existing Documentation
- **@doc/development-patterns.md**: Cross-project development workflows
- **@doc/git-workflow.md**: Submodule and commit workflows  
- **@doc/commit-guide.md**: Complete emoji and convention reference

## Optimization Results

### Token Efficiency
- **Before**: 261 total lines (Root: 41, API: 149, Web: 71)
- **After**: 167 total lines (Root: 26, API: 83, Web: 58)
- **Reduction**: 36% fewer lines, ~2,500-3,000 tokens saved per session

### Content Organization
- **Removed**: Meta-maintenance instructions, dated session learnings
- **Extracted**: Verbose code examples to dedicated documentation files
- **Consolidated**: Redundant information across projects
- **Streamlined**: Essential commands and quick patterns only

## Validation Checklist

### Essential Functions Maintained ✅
- [x] Directory check: `pwd` command guidance
- [x] Port checks: `lsof -i :3000` and `lsof -i :5173`
- [x] Package manager enforcement: bun vs pnpm
- [x] Submodule workflow: absolute paths, commit order
- [x] Navigation commands: `/project:workdir @api|@web-back-office|@root`

### Cross-References Working ✅
- [x] @doc/api-patterns.md - Detailed NestJS patterns
- [x] @doc/nestjs-testing.md - Testing strategies
- [x] @doc/react-patterns.md - React development patterns
- [x] @doc/development-patterns.md - General workflows
- [x] @doc/git-workflow.md - Submodule management

### Project-Specific Guidance Preserved ✅
- [x] API: NestJS + Prisma + Bun specific commands and patterns
- [x] Web: React + TanStack Query + PNPM specific guidance
- [x] Root: Monorepo navigation and cross-project rules

## Usage Guidelines

### When Working in Root Directory
- Loads: Root CLAUDE.md (26 lines)
- Focus: Monorepo navigation, git workflows, cross-project rules

### When Working in /api Directory  
- Loads: Root CLAUDE.md + API CLAUDE.md (26 + 83 = 109 lines)
- Focus: NestJS development, testing, error handling

### When Working in /web-back-office Directory
- Loads: Root CLAUDE.md + Web CLAUDE.md (26 + 58 = 84 lines)  
- Focus: React development, TanStack Query, component patterns

## Memory Maintenance Rules

### Auto-Update Schedule
- **Root**: Review every 10 sessions
- **Projects**: Review every 5 sessions
- **Documentation**: Update when patterns change

### Content Guidelines
- **Keep in Memory**: High-frequency commands, critical warnings, quick patterns
- **Move to @doc/**: Detailed examples, comprehensive guides, reference material
- **Remove**: Dated content, maintenance instructions, redundant information

### Size Limits
- **Root**: <30 lines (currently 26)
- **API**: <100 lines (currently 83)
- **Web**: <80 lines (currently 58)

## Future Optimization Opportunities

### If Memory Files Grow Again
1. **Extract More Examples**: Move remaining code snippets to @doc/ files
2. **Compress Commands**: Use more compact command listings
3. **Consolidate Rules**: Merge similar guidelines into single statements

### New Projects
- **Web Portal**: When implemented, create optimized CLAUDE.md (<60 lines)
- **Mobile Apps**: Follow same pattern with project-specific guidance
- **Shared Libraries**: Consider separate memory files for complex shared code

This optimized structure provides maximum efficiency while maintaining full functionality and clear development guidance.