# NestJS Testing Patterns

## Unit Testing Patterns

### Service Unit Tests
```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { PrismaService } from '~/libs/prisma';
import { ServiceName } from './service-name.service';
import { mockDeep, DeepMockProxy } from 'jest-mock-extended';
import { PrismaClient } from '@prisma/client';

describe('ServiceName', () => {
  let service: ServiceName;
  let prisma: DeepMockProxy<PrismaClient>;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        ServiceName,
        PrismaService,
      ],
    })
      .overrideProvider(PrismaService)
      .useValue(mockDeep<PrismaClient>())
      .compile();

    service = module.get<ServiceName>(ServiceName);
    prisma = module.get(PrismaService);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  describe('findOne', () => {
    it('should return a resource when found', async () => {
      const mockResource = { id: '1', name: 'Test Resource' };
      prisma.resource.findUnique.mockResolvedValue(mockResource);

      const result = await service.findOne('1');

      expect(result).toEqual(mockResource);
      expect(prisma.resource.findUnique).toHaveBeenCalledWith({
        where: { id: '1' },
      });
    });

    it('should throw NotFoundException when resource not found', async () => {
      prisma.resource.findUnique.mockResolvedValue(null);

      await expect(service.findOne('1')).rejects.toThrow(CodedNotFoundException);
      expect(prisma.resource.findUnique).toHaveBeenCalledWith({
        where: { id: '1' },
      });
    });
  });

  describe('create', () => {
    it('should create and return a new resource', async () => {
      const createDto = { name: 'New Resource' };
      const mockCreated = { id: '1', ...createDto };
      prisma.resource.create.mockResolvedValue(mockCreated);

      const result = await service.create(createDto);

      expect(result).toEqual(mockCreated);
      expect(prisma.resource.create).toHaveBeenCalledWith({
        data: createDto,
      });
    });

    it('should handle P2002 constraint violation', async () => {
      const createDto = { name: 'Duplicate Resource' };
      const error = new Error('Unique constraint failed');
      error.code = 'P2002';
      prisma.resource.create.mockRejectedValue(error);

      await expect(service.create(createDto)).rejects.toThrow(CodedConflictException);
    });
  });
});
```

### Controller Unit Tests
```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { ResourceController } from './resource.controller';
import { ResourceService } from './resource.service';

describe('ResourceController', () => {
  let controller: ResourceController;
  let service: ResourceService;

  const mockService = {
    findAll: jest.fn(),
    findOne: jest.fn(),
    create: jest.fn(),
    update: jest.fn(),
    remove: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [ResourceController],
      providers: [
        {
          provide: ResourceService,
          useValue: mockService,
        },
      ],
    }).compile();

    controller = module.get<ResourceController>(ResourceController);
    service = module.get<ResourceService>(ResourceService);
  });

  afterEach(() => {
    jest.clearAllMocks();
  });

  describe('findAll', () => {
    it('should return an array of resources', async () => {
      const mockResources = { items: [], total: 0, page: 1, limit: 10 };
      mockService.findAll.mockResolvedValue(mockResources);

      const result = await controller.findAll({});

      expect(result).toEqual(mockResources);
      expect(service.findAll).toHaveBeenCalledWith({});
    });
  });

  describe('create', () => {
    it('should create a new resource', async () => {
      const createDto = { name: 'Test Resource' };
      const mockCreated = { id: '1', ...createDto };
      mockService.create.mockResolvedValue(mockCreated);

      const result = await controller.create(createDto);

      expect(result).toEqual(mockCreated);
      expect(service.create).toHaveBeenCalledWith(createDto);
    });
  });
});
```

## E2E Testing Patterns

### Test Setup
```typescript
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import { PrismaService } from '~/libs/prisma';
import { setupTestApp, cleanupTestData, createAuthToken } from '~/test/setup';
import { adminFixture } from '~/test/fixtures';
import request from 'supertest';

describe('Resource E2E Tests', () => {
  let app: INestApplication;
  let prisma: PrismaService;
  let authToken: string;

  beforeAll(async () => {
    ({ app, prisma } = await setupTestApp());
    authToken = await createAuthToken(app, adminFixture);
  });

  afterAll(async () => {
    await app.close();
  });

  afterEach(async () => {
    await cleanupTestData(prisma);
  });

  describe('/resources (GET)', () => {
    it('should return empty list when no resources exist', async () => {
      const response = await request(app.getHttpServer())
        .get('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);

      expect(response.body).toEqual({
        items: [],
        total: 0,
        page: 1,
        limit: 10,
      });
    });

    it('should return list of resources', async () => {
      // Setup test data
      const resource = await prisma.resource.create({
        data: { name: 'Test Resource' },
      });

      const response = await request(app.getHttpServer())
        .get('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);

      expect(response.body.items).toHaveLength(1);
      expect(response.body.items[0]).toMatchObject({
        id: resource.id,
        name: resource.name,
      });
    });

    it('should require authentication', async () => {
      await request(app.getHttpServer())
        .get('/resources')
        .expect(401);
    });
  });

  describe('/resources (POST)', () => {
    it('should create a new resource', async () => {
      const createDto = { name: 'New Resource' };

      const response = await request(app.getHttpServer())
        .post('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .send(createDto)
        .expect(201);

      expect(response.body).toMatchObject(createDto);
      expect(response.body.id).toBeDefined();

      // Verify in database
      const created = await prisma.resource.findUnique({
        where: { id: response.body.id },
      });
      expect(created).toMatchObject(createDto);
    });

    it('should validate required fields', async () => {
      await request(app.getHttpServer())
        .post('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .send({})
        .expect(400);
    });

    it('should handle duplicate resource names', async () => {
      const createDto = { name: 'Duplicate Resource' };

      // Create first resource
      await request(app.getHttpServer())
        .post('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .send(createDto)
        .expect(201);

      // Try to create duplicate
      const response = await request(app.getHttpServer())
        .post('/resources')
        .set('Authorization', `Bearer ${authToken}`)
        .send(createDto)
        .expect(409);

      expect(response.body.code).toBe('RESOURCE_CONFLICT');
    });
  });

  describe('/resources/:id (GET)', () => {
    it('should return resource by id', async () => {
      const resource = await prisma.resource.create({
        data: { name: 'Test Resource' },
      });

      const response = await request(app.getHttpServer())
        .get(`/resources/${resource.id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200);

      expect(response.body).toMatchObject({
        id: resource.id,
        name: resource.name,
      });
    });

    it('should return 404 for non-existent resource', async () => {
      await request(app.getHttpServer())
        .get('/resources/non-existent-id')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(404);
    });
  });

  describe('/resources/:id (PUT)', () => {
    it('should update existing resource', async () => {
      const resource = await prisma.resource.create({
        data: { name: 'Original Name' },
      });

      const updateDto = { name: 'Updated Name' };

      const response = await request(app.getHttpServer())
        .put(`/resources/${resource.id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .send(updateDto)
        .expect(200);

      expect(response.body.name).toBe(updateDto.name);

      // Verify in database
      const updated = await prisma.resource.findUnique({
        where: { id: resource.id },
      });
      expect(updated.name).toBe(updateDto.name);
    });
  });

  describe('/resources/:id (DELETE)', () => {
    it('should delete existing resource', async () => {
      const resource = await prisma.resource.create({
        data: { name: 'To Delete' },
      });

      await request(app.getHttpServer())
        .delete(`/resources/${resource.id}`)
        .set('Authorization', `Bearer ${authToken}`)
        .expect(204);

      // Verify deleted from database
      const deleted = await prisma.resource.findUnique({
        where: { id: resource.id },
      });
      expect(deleted).toBeNull();
    });
  });
});
```

## Test Utilities

### Setup Test App
```typescript
// test/setup/test-database.ts
export async function setupTestApp(): Promise<{
  app: INestApplication;
  prisma: PrismaService;
}> {
  const moduleFixture: TestingModule = await Test.createTestingModule({
    imports: [AppModule],
  }).compile();

  const app = moduleFixture.createNestApplication();
  
  // Apply same middleware as main app
  app.useGlobalPipes(new ValidationPipe());
  app.useGlobalFilters(new GlobalExceptionFilter());
  
  await app.init();

  const prisma = app.get<PrismaService>(PrismaService);
  
  return { app, prisma };
}
```

### Auth Test Helper
```typescript
// test/setup/auth-test-helper.ts
export async function createAuthToken(
  app: INestApplication,
  adminData: any
): Promise<string> {
  const prisma = app.get<PrismaService>(PrismaService);
  
  // Create test admin
  const admin = await prisma.admin.create({
    data: adminData,
  });

  // Login to get token
  const response = await request(app.getHttpServer())
    .post('/auth/login')
    .send({
      username: admin.username,
      password: adminData.password, // Use original password
    })
    .expect(200);

  return response.body.accessToken;
}
```

### Cleanup Test Data
```typescript
// test/setup/test-helpers.ts
export async function cleanupTestData(prisma: PrismaService): Promise<void> {
  const tablenames = await prisma.$queryRaw<
    Array<{ tablename: string }>
  >`SELECT tablename FROM pg_tables WHERE schemaname='public'`;

  const tables = tablenames
    .map(({ tablename }) => tablename)
    .filter((name) => name !== '_prisma_migrations')
    .map((name) => `"public"."${name}"`)
    .join(', ');

  try {
    await prisma.$executeRawUnsafe(`TRUNCATE TABLE ${tables} CASCADE;`);
  } catch (error) {
    console.log({ error });
  }
}
```

## Testing Best Practices

### 1. Test Structure
- **Unit Tests**: Test individual methods in isolation
- **Integration Tests**: Test module interactions
- **E2E Tests**: Test complete request/response cycles

### 2. Mock Strategy
- Use `mockDeep` for Prisma client mocking
- Mock external dependencies completely
- Use TestContainers for real database testing

### 3. Data Management
- Clean up test data after each test
- Use fixtures for consistent test data
- Isolate tests from each other

### 4. Authentication Testing
- Create test tokens for authenticated endpoints
- Test both authenticated and unauthenticated scenarios
- Use consistent test user data

### 5. Error Scenario Testing
- Test validation errors
- Test business rule violations
- Test database constraint violations
- Test authentication/authorization failures