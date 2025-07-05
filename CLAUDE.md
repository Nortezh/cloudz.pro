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