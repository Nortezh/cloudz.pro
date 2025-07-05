# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Important: Project-Specific Memory Files

This monorepo uses separate CLAUDE.md files for each project:
- `/api/CLAUDE.md` - API-specific instructions (Bun, NestJS patterns)
- `/web-back-office/CLAUDE.md` - Frontend-specific instructions (PNPM, React patterns)
- `/web-portal/CLAUDE.md` - Portal-specific instructions (when implemented)

Claude Code automatically loads the appropriate memory file based on your working directory.

## Repository Structure

This is a monorepo containing three projects:
- **api**: Enterprise NestJS backend with PostgreSQL/Prisma
- **web-back-office**: React Router v7 admin interface
- **web-portal**: Customer-facing portal (not yet implemented)

## Monorepo-Wide Standards

### Commit Standards

Use Conventional Commits with Gitmoji:
```
<type>: <emoji> <description>

Examples:
feat: ✨ add user authentication
fix: 🐛 resolve login validation error
docs: 📝 update API documentation
refactor: ♻️ simplify user service logic
test: ✅ add user controller tests
```

See COMMIT_GUIDE.md for full emoji reference.

**Important**: Do NOT include "🤖 Generated with Claude Code" or similar AI attribution in commit messages.

### Git Submodule Handling

When working with submodules:
1. Commit changes within each submodule first
2. Push submodule changes
3. Then update parent repository's submodule references
4. Cannot directly `git add` files inside submodules from parent directory

### Cross-Project Patterns

1. **TypeScript Everywhere**: All projects use TypeScript with strict mode
2. **JWT Authentication**: Shared authentication pattern across API and frontends
3. **Environment Variables**: Use `.env` files (never commit them)
4. **Error Handling**: Comprehensive error handling with meaningful messages
5. **Testing**: Write tests for critical functionality

### Workspace Commands

When working across multiple projects:
```bash
# From root directory
cd api && bun install
cd ../web-back-office && pnpm install

# Or use workspace tools if configured
```

## Development Workflow

### Directory Check Before Running Commands

**CRITICAL**: This is a monorepo with git submodules. Always check your current directory before running any command:
```bash
pwd  # Always run this first to verify your location
```

- Root directory (`/cloudz.pro`): For git operations on the parent repository
- `/api` directory: For Bun commands and API development
- `/web-back-office` directory: For PNPM commands and frontend development
- `/web-portal` directory: For portal development (when implemented)

Common mistakes to avoid:
- Running `bun` commands in the web directories (use `pnpm` instead)
- Running `pnpm` commands in the api directory (use `bun` instead)
- Trying to commit submodule changes from the parent directory
- Running development servers from the wrong directory

### Port Check Before Starting Services

**Important**: Always check if ports are already in use before starting development servers:
- API (port 3000): Run `lsof -i :3000` or `netstat -an | grep 3000` before `bun start:dev`
- Web Back Office (port 5173): Run `lsof -i :5173` or `netstat -an | grep 5173` before `pnpm dev`

If a port is already in use, either:
1. Stop the existing service: `kill -9 $(lsof -t -i:PORT)`
2. Or use a different terminal/tab for the new service

1. **Feature Development**:
   - Create feature branch from main
   - Work in relevant project directory
   - Follow project-specific patterns
   - Test thoroughly
   - Create PR with clear description

2. **Cross-Project Changes**:
   - Update API first
   - Update frontend(s) to match
   - Test integration between projects
   - Document any breaking changes

## Performance Targets

- API response time: < 50ms
- Frontend initial load: < 3s
- Build time: < 2 minutes per project

## Security Considerations

- Never commit secrets or API keys
- Use environment variables for configuration
- Keep dependencies updated
- Follow OWASP best practices