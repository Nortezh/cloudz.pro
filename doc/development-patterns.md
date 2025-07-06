# Development Patterns

## Auto-Update Memory Rules

### Root CLAUDE.md Auto-Update (Every 10 sessions)
- Check file size against 100-line limit
- Remove instructions unused in last 10 sessions
- Consolidate similar rules into single statements
- Move detailed content to @doc/ files if size exceeds 20% growth
- Run validation tests: directory check, port check, commit format, external docs

### Project-Specific CLAUDE.md Auto-Update (Every 5 sessions)
- API: Keep under 200 lines, remove unused patterns
- Web Back Office: Keep under 80 lines, essential commands only
- Web Portal: Keep under 60 lines, optimize for future implementation
- Cross-reference detailed guides instead of duplicating content

### Auto-Update Workflow
1. **Session Counter**: Track usage of each instruction/pattern
2. **Threshold Check**: Mark unused items after specified sessions
3. **Cleanup Phase**: Remove marked items and consolidate similar rules
4. **Validation**: Test critical functionality after optimization
5. **Documentation**: Update @doc/ files with moved detailed content

### Validation and Error Handling Best Practices (Session Learning: 2025-07-06)
1. **Validation Layer Separation**: Keep DTO validation minimal (types only), use service validation for business rules
2. **Security in Error Handling**: Never expose sensitive data in CodedConflictException details field
3. **P2002 Pattern**: All Prisma constraint violations must use CodedConflictException with proper error codes
4. **Testing Strategy**: Use e2e tests with testcontainers to validate error handling with real database

### Investigation Best Practices (Session Learning: 2025-07-05)
1. **Always Explore First**: Use Read/LS tools to examine existing patterns before creating files
2. **Pattern Recognition**: Directory contents and file extensions reveal established conventions
3. **Assumption Prevention**: Never assume file formats or structures without verification
4. **Context Gathering**: Examine similar files in the same directory for guidance

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
- Use coded exceptions for consistent error responses with i18n support
- **Security**: Never expose sensitive data (emails, IDs) in error details
- **P2002 Pattern**: Handle Prisma constraint violations with CodedConflictException
- Consistent error response format:
  ```json
  {
    "statusCode": 409,
    "code": "RESOURCE_ALREADY_EXISTS",
    "message": "Resource already exists",
    "details": {}, // NO sensitive data here
    "timestamp": "2025-07-06T..."
  }
  ```
- Separate validation layers: DTO (types) vs Service (business rules)
- Log errors with context, return user-friendly messages

### Validation Strategy (Session Learning: 2025-07-06)
- **DTO Layer**: Minimal validation for types and basic transformation only
  ```typescript
  @IsString({ message: 'Name must be a string' })
  @Transform(({ value }) => value?.trim())
  name: string;
  ```
- **Service Layer**: Business rule validation with coded exceptions
  ```typescript
  if (!isValidFormat(name)) {
    throw new CodedBadRequestException(
      ERROR_CODES.INVALID_FORMAT,
      {},
      'Invalid format provided'
    );
  }
  ```
- **P2002 Error Pattern**: Standardized database constraint handling
  ```typescript
  if (error.code === 'P2002') {
    throw new CodedConflictException(
      ERROR_CODES.RESOURCE_CONFLICT,
      {}, // Never include sensitive data
      'Resource already exists'
    );
  }
  ```

### Testing Strategy
- Write tests for critical functionality
- Unit tests for services and utilities
- E2E tests with testcontainers for real database validation
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