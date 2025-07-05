# Development Patterns

## Cross-Project Standards

### TypeScript Configuration
- All projects use TypeScript with strict mode enabled
- Consistent tsconfig.json settings across projects
- Shared type definitions where applicable

### Authentication Pattern
- JWT-based authentication across all services
- Tokens stored in localStorage (frontend)
- 24-hour token expiration
- Consistent auth headers: `Authorization: Bearer <token>`

### Environment Variables
- Use `.env` files for configuration
- NEVER commit `.env` files
- Document required variables in `.env.example`
- Access via `process.env.VARIABLE_NAME`

### Error Handling
- Comprehensive error handling with meaningful messages
- Consistent error response format:
  ```json
  {
    "statusCode": 400,
    "message": "Descriptive error message",
    "error": "BadRequest"
  }
  ```
- Log errors with context
- Return user-friendly error messages

### Testing Strategy
- Write tests for critical functionality
- Unit tests for services and utilities
- E2E tests for API endpoints
- Integration tests for cross-service communication

## Feature Development Workflow

1. **Create Feature Branch**
   - Branch from main: `git checkout -b feature/description`
   - Use descriptive branch names

2. **Development Process**
   - Work in relevant project directory
   - Follow project-specific patterns (see individual CLAUDE.md)
   - Write tests alongside code
   - Keep commits atomic and meaningful

3. **Testing**
   - Run unit tests: `bun test` (api) or `pnpm test` (web)
   - Run E2E tests if applicable
   - Test integration between services
   - Verify no regression

4. **Pull Request**
   - Clear PR description
   - Link related issues
   - Include test plan
   - Request reviews from team

## Cross-Project Changes

When changes span multiple projects:

1. **API First**
   - Implement backend changes
   - Update API documentation
   - Deploy to staging

2. **Frontend Updates**
   - Update API client types
   - Implement UI changes
   - Test against staging API

3. **Integration Testing**
   - Test full flow end-to-end
   - Verify error handling
   - Check performance impact

4. **Documentation**
   - Update API docs
   - Document breaking changes
   - Update deployment guides

## Code Style Guidelines

### General
- 2-space indentation
- Consistent naming conventions
- Descriptive variable names
- Avoid magic numbers

### TypeScript
- Use interfaces over types when possible
- Explicit return types for functions
- Avoid `any` type
- Use enums for constants

### Comments
- Only add comments when necessary
- Explain "why" not "what"
- Keep comments up-to-date
- Use JSDoc for public APIs