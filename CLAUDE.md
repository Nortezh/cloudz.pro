# CLAUDE.md

## Monorepo: Git Submodules
`pwd` first | Root: git ops | `/api`: bun only | `/web-back-office`: pnpm only

## Navigation
`/project:workdir @api|@web-back-office|@root`

## Commits  
`<type>: <emoji> <description>` - NO AI attribution
Workflow: submodule first → parent repo (absolute paths)

## Development Rules
1. KISS: functions <20 lines, <2 nesting
2. Tests: 100% pass before completion  
3. Package managers: `/api`→bun, `/web-*`→pnpm (STRICT)
4. Performance: API <50ms, Frontend <3s
5. Security: NEVER commit secrets

## Ports
API: 3000 (`lsof -i :3000`) | Web: 5173 (`lsof -i :5173`)

## Tech Stack
All: TypeScript, JWT, `.env` | API: NestJS+Prisma+Bun | Web: React Router v7+Ant Design+PNPM

## Resources
@doc/development-patterns.md | @doc/git-workflow.md | @doc/commit-guide.md