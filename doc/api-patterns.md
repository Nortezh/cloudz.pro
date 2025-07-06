# API Development Patterns

## NestJS Service Pattern

### Standard Service Structure
```typescript
@Injectable()
export class ServiceName {
  private readonly logger = new Logger(ServiceName.name);
  
  constructor(private prisma: PrismaService) {}
  
  async methodName() {
    // Pre-validate with coded exceptions
    const existing = await this.prisma.model.findUnique({ where: { id } });
    if (!existing) {
      throw new CodedNotFoundException(
        ERROR_CODES.RESOURCE_NOT_FOUND,
        { id },
        'Resource not found'
      );
    }
    
    try {
      // Use transactions for multi-step operations
      return this.prisma.$transaction(async (tx) => {
        // Implementation
      });
    } catch (error) {
      // Handle P2002 constraint violations
      if (error.code === 'P2002') {
        throw new CodedConflictException(
          ERROR_CODES.RESOURCE_CONFLICT,
          {}, // NEVER include sensitive data
          'Resource conflict occurred'
        );
      }
      throw error;
    }
  }
}
```

### Error Handling Patterns
```typescript
// Pre-validation pattern
const existing = await this.prisma.model.findUnique({ where: { id } });
if (!existing) {
  throw new CodedNotFoundException(
    ERROR_CODES.RESOURCE_NOT_FOUND,
    { id },
    'Resource not found'
  );
}

// P2002 constraint violation handling
try {
  return await this.prisma.model.create({ data });
} catch (error) {
  if (error.code === 'P2002') {
    throw new CodedConflictException(
      ERROR_CODES.RESOURCE_CONFLICT,
      {}, // NEVER include sensitive data
      'Resource already exists'
    );
  }
  throw error;
}

// Business rule validation
if (!isValidFormat(name)) {
  throw new CodedBadRequestException(
    ERROR_CODES.INVALID_FORMAT,
    {},
    'Invalid format provided'
  );
}
```

## Controller Pattern

### Standard Controller Structure
```typescript
@Controller('resource')
@UseGuards(JwtAuthGuard)
@ApiTags('resources')
export class ResourceController {
  constructor(private readonly service: ResourceService) {}
  
  @Get()
  @ApiOperation({ summary: 'List resources' })
  @ApiResponse({ status: 200, description: 'Success', type: ResourceListResponseDto })
  async findAll(@Query() filter: ResourceFilterDto) {
    return this.service.findAll(filter);
  }

  @Post()
  @ApiOperation({ summary: 'Create resource' })
  @ApiResponse({ status: 201, description: 'Created', type: ResourceResponseDto })
  async create(@Body() createDto: CreateResourceDto) {
    return this.service.create(createDto);
  }

  @Get(':id')
  @ApiOperation({ summary: 'Get resource by ID' })
  @ApiResponse({ status: 200, description: 'Found', type: ResourceResponseDto })
  async findOne(@Param('id') id: string) {
    return this.service.findOne(id);
  }

  @Put(':id')
  @ApiOperation({ summary: 'Update resource' })
  @ApiResponse({ status: 200, description: 'Updated', type: ResourceResponseDto })
  async update(@Param('id') id: string, @Body() updateDto: UpdateResourceDto) {
    return this.service.update(id, updateDto);
  }

  @Delete(':id')
  @ApiOperation({ summary: 'Delete resource' })
  @ApiResponse({ status: 204, description: 'Deleted' })
  async remove(@Param('id') id: string) {
    await this.service.remove(id);
  }
}
```

## DTO Pattern

### Create DTO
```typescript
export class CreateResourceDto {
  @ApiProperty({ description: 'Resource name' })
  @IsString({ message: 'Name must be a string' })
  @Transform(({ value }) => value?.trim())
  name: string;

  @ApiProperty({ description: 'Resource description', required: false })
  @IsString({ message: 'Description must be a string' })
  @IsOptional()
  @Transform(({ value }) => value?.trim())
  description?: string;

  @ApiProperty({ description: 'Resource status' })
  @IsEnum(ResourceStatus, { message: 'Status must be a valid enum value' })
  status: ResourceStatus;
}
```

### Filter DTO
```typescript
export class ResourceFilterDto {
  @ApiProperty({ description: 'Search term', required: false })
  @IsString()
  @IsOptional()
  search?: string;

  @ApiProperty({ description: 'Status filter', required: false })
  @IsEnum(ResourceStatus)
  @IsOptional()
  status?: ResourceStatus;

  @ApiProperty({ description: 'Page number', required: false, default: 1 })
  @Type(() => Number)
  @IsInt({ message: 'Page must be an integer' })
  @Min(1, { message: 'Page must be at least 1' })
  @IsOptional()
  page?: number = 1;

  @ApiProperty({ description: 'Items per page', required: false, default: 10 })
  @Type(() => Number)
  @IsInt({ message: 'Limit must be an integer' })
  @Min(1, { message: 'Limit must be at least 1' })
  @Max(100, { message: 'Limit must not exceed 100' })
  @IsOptional()
  limit?: number = 10;
}
```

### Response DTO
```typescript
export class ResourceResponseDto {
  @ApiProperty({ description: 'Resource ID' })
  id: string;

  @ApiProperty({ description: 'Resource name' })
  name: string;

  @ApiProperty({ description: 'Resource description' })
  description: string;

  @ApiProperty({ description: 'Resource status' })
  status: ResourceStatus;

  @ApiProperty({ description: 'Creation timestamp' })
  createdAt: Date;

  @ApiProperty({ description: 'Last update timestamp' })
  updatedAt: Date;
}

export class ResourceListResponseDto {
  @ApiProperty({ description: 'List of resources', type: [ResourceResponseDto] })
  items: ResourceResponseDto[];

  @ApiProperty({ description: 'Total number of items' })
  total: number;

  @ApiProperty({ description: 'Current page' })
  page: number;

  @ApiProperty({ description: 'Items per page' })
  limit: number;
}
```

## Module Pattern

```typescript
@Module({
  imports: [PrismaModule],
  controllers: [ResourceController],
  providers: [ResourceService],
  exports: [ResourceService],
})
export class ResourceModule {}
```

## Key Architecture Principles

### 1. Dependency Injection
- Use constructor injection for services
- Inject PrismaService for database operations
- Use Logger for structured logging

### 2. Error Handling
- Use coded exceptions for consistent API responses
- Pre-validate to provide better error messages
- Handle Prisma constraint violations (P2002)
- Never expose sensitive data in error details

### 3. Validation Strategy
- **DTO Layer**: Type validation and basic transformation
- **Service Layer**: Business rule validation with coded exceptions
- **Database Layer**: Constraint handling with meaningful errors

### 4. Performance Patterns
- Use transactions for multi-step operations
- Implement pagination for list endpoints
- Use database indexes for frequent queries
- Pre-validate before expensive operations

### 5. Security Patterns
- JWT authentication with guards
- Validate all input data
- Log operations with context
- Never log sensitive information

## Testing Patterns

See @doc/nestjs-testing.md for comprehensive testing strategies.