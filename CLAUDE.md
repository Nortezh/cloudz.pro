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

1. **IMPORTANT: Wrong package manager = build failure**
   - `/api` → `bun` ONLY
   - `/web-*` → `pnpm` ONLY

2. **Submodule commits require absolute paths:**
   ```bash
   cd /full/path/to/submodule && git commit && git push
   cd /full/path/to/parent && git add submodule && git commit
   ```

3. **Performance targets:**
   - API: < 50ms response
   - Frontend: < 3s initial load
   - Build: < 2 minutes

4. **Security: NEVER commit secrets or API keys**

## Additional Resources (Reference only when needed)
- Detailed patterns: @doc/development-patterns.md
- Git workflow: @doc/git-workflow.md
- Full commit guide: @doc/commit-guide.md
- Architecture details: See project-specific CLAUDE.md files