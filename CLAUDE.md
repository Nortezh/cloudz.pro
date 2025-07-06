# CLAUDE.md

## Memory Optimization Rules

**IMPORTANT: Keep this file efficient to save tokens**
- If adding content increases size by >20%, move details to @doc/ files
- If this file exceeds 100 lines, refactor immediately
- Remove any instruction that hasn't been used in last 10 sessions
- Consolidate similar rules into single concise statements

**Auto-Update Rules:**
- Every 10 sessions: Auto-optimize root CLAUDE.md
- Every 5 sessions: Review project-specific CLAUDE.md files
- Auto-remove unused instructions older than 10 sessions
- Auto-consolidate similar rules into single statements

**Validation Test After Optimization:**
1. Directory check: `pwd` command usage
2. Port check: Verify port checking before services
3. Commit format: Test proper format application
4. External docs: Confirm @doc/ references work

**Critical Session Learning (2025-07-06):**
- **KISS Principle**: Code must be "stupid simple" - avoid over-engineering (26% complexity reduction achieved)
- **Test Gate**: ALL unit and E2E tests MUST pass 100% before task completion
- **Configuration Conflicts**: Separate test directories prevent tool conflicts (`app/__tests__/` vs `e2e/`)
- **Over-Engineering Detection**: Remove complex solutions early (Error Context Provider was unnecessary)
- **React Testing**: Wrap state updates in `act()` for proper async testing patterns
- **Workflow Enhancement**: Created `/implement` command with built-in quality gates
- **Tool Effectiveness**: Parallel agents excellent for research, Read/LS essential for exploration

**Previous Session (2025-07-05):**
- **NEVER assume file formats** - Always investigate existing patterns first
- **Use Read/LS tools** to explore codebase conventions before creating files
- **Pattern Recognition**: Directory contents reveal established standards

## CRITICAL: Monorepo with Git Submodules

**YOU MUST check current directory before ANY command:**
```bash
pwd  # ALWAYS run this first
```

- Root (`/cloudz.pro`): Git operations on parent repo
- `/api`: Bun commands ONLY 
- `/web-back-office`: PNPM commands ONLY
- `/web-portal`: Portal development (when implemented)

**Each project has its own CLAUDE.md - Claude Code loads based on working directory.**

## Quick Reference

### Claude Code Commands (Custom Navigation)
- `/project:workdir @api` - Navigate to API project directory
- `/project:workdir @web-back-office` - Navigate to Web Back Office directory  
- `/project:workdir @root` - Navigate to monorepo root directory

### Commits
Format: `<type>: <emoji> <description>`
- **Common**: `feat: ✨`, `fix: 🐛`, `docs: 📝`, `chore: 🔧`, `refactor: ♻️`
- **CRITICAL**: NEVER include AI attribution ("🤖 Generated with", "Co-Authored-By: Claude")
- **Submodules**: Use absolute paths (`cd /full/path/to/submodule`)
- Commit workflow: submodule first → parent repo second

### Ports (Check before starting)
- API: 3000 - `lsof -i :3000` before `bun start:dev`
- Web: 5173 - `lsof -i :5173` before `pnpm dev`

### Tech Stack
- **All projects**: TypeScript strict mode, JWT auth, `.env` files
- **API**: NestJS, PostgreSQL, Prisma, Bun
- **Web Back Office**: React Router v7, Ant Design, TanStack Query, PNPM

## Development Rules

1. **KISS Principle (Mandatory)**
   - Code must be "stupid simple" - if junior dev can't understand in 5 min, simplify
   - Functions <20 lines, avoid nesting >2 levels, self-documenting names
   - Only create abstractions when pattern repeats 3+ times

2. **Test Gates (Mandatory)**
   - `pnpm test && pnpm test:e2e` MUST pass 100% before task completion
   - Zero test failures allowed - fix immediately, don't proceed
   - All imported functions must be mocked completely

3. **Package Manager (Critical)**
   - `/api` → `bun` ONLY, `/web-*` → `pnpm` ONLY
   - Wrong package manager = build failure

4. **Submodule Workflow**
   ```bash
   cd /full/path/to/submodule && git commit && git push
   cd /full/path/to/parent && git add submodule && git commit
   ```

5. **Performance & Security**
   - API: <50ms, Frontend: <3s load, Build: <2min
   - NEVER commit secrets or API keys

## Additional Resources (Reference only when needed)
- Detailed patterns: @doc/development-patterns.md
- Git workflow: @doc/git-workflow.md
- Full commit guide: @doc/commit-guide.md
- Architecture details: See project-specific CLAUDE.md files