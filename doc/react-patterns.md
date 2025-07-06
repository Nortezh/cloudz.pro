# React Development Patterns

## Component Patterns

### Basic Component Structure
```tsx
interface ComponentProps {
  prop: string;
  optionalProp?: number;
  children?: React.ReactNode;
}

export default function ComponentName({ prop, optionalProp, children }: ComponentProps) {
  return (
    <div data-testid="component-name" className="component-wrapper">
      <h1>{prop}</h1>
      {optionalProp && <span>{optionalProp}</span>}
      {children}
    </div>
  );
}
```

### Page Component Pattern
```tsx
import { StandardPageTemplate } from '~/components/StandardPageTemplate';

export default function PageName() {
  return (
    <StandardPageTemplate 
      header={{ 
        title: "Page Title",
        subtitle: "Optional subtitle",
        actions: [
          <Button key="action" type="primary">Action</Button>
        ]
      }}
    >
      <div className="page-content">
        {/* Page content here */}
      </div>
    </StandardPageTemplate>
  );
}
```

### Form Component Pattern
```tsx
import { Form, Input, Button, message } from 'antd';
import { useCreateResource } from '~/lib/api/hooks';

interface FormData {
  name: string;
  description?: string;
}

export default function ResourceForm() {
  const [form] = Form.useForm<FormData>();
  const createMutation = useCreateResource();

  const handleSubmit = async (values: FormData) => {
    try {
      await createMutation.mutateAsync(values);
      message.success('Resource created successfully');
      form.resetFields();
    } catch (error) {
      message.error(getErrorMessage(error));
    }
  };

  return (
    <Form
      form={form}
      layout="vertical"
      onFinish={handleSubmit}
      data-testid="resource-form"
    >
      <Form.Item
        name="name"
        label="Name"
        rules={[{ required: true, message: 'Name is required' }]}
      >
        <Input placeholder="Enter resource name" />
      </Form.Item>

      <Form.Item name="description" label="Description">
        <Input.TextArea placeholder="Enter description" />
      </Form.Item>

      <Form.Item>
        <Button 
          type="primary" 
          htmlType="submit"
          loading={createMutation.isPending}
        >
          Create Resource
        </Button>
      </Form.Item>
    </Form>
  );
}
```

## TanStack Query Hooks

### Query Hook Pattern
```tsx
import { useQuery } from '@tanstack/react-query';
import { api } from '~/lib/api/config';

export function useResources(filter?: ResourceFilter) {
  return useQuery({
    queryKey: ['resources', filter],
    queryFn: () => api.get('/resources', { params: filter }),
    staleTime: 5 * 60 * 1000, // 5 minutes
    refetchOnWindowFocus: false,
  });
}

export function useResource(id: string) {
  return useQuery({
    queryKey: ['resources', id],
    queryFn: () => api.get(`/resources/${id}`),
    enabled: !!id,
  });
}
```

### Mutation Hook Pattern
```tsx
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { api } from '~/lib/api/config';
import { message } from 'antd';

export function useCreateResource() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (data: CreateResourceRequest) => 
      api.post('/resources', data),
    onSuccess: (newResource) => {
      // Invalidate and refetch resources list
      queryClient.invalidateQueries({ queryKey: ['resources'] });
      
      // Optimistically update cache with new resource
      queryClient.setQueryData(['resources', newResource.id], newResource);
      
      message.success('Resource created successfully');
    },
    onError: (error) => {
      message.error(getErrorMessage(error));
    },
  });
}

export function useUpdateResource() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: ({ id, data }: { id: string; data: UpdateResourceRequest }) =>
      api.put(`/resources/${id}`, data),
    onSuccess: (updatedResource, { id }) => {
      // Update specific resource in cache
      queryClient.setQueryData(['resources', id], updatedResource);
      
      // Invalidate list to reflect changes
      queryClient.invalidateQueries({ queryKey: ['resources'] });
    },
  });
}

export function useDeleteResource() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (id: string) => api.delete(`/resources/${id}`),
    onSuccess: (_, id) => {
      // Remove from cache
      queryClient.removeQueries({ queryKey: ['resources', id] });
      
      // Invalidate list
      queryClient.invalidateQueries({ queryKey: ['resources'] });
      
      message.success('Resource deleted successfully');
    },
  });
}
```

## Error Handling Patterns

### API Error Utilities
```tsx
import { AxiosError } from 'axios';

export interface ApiError {
  statusCode: number;
  code: string;
  message: string;
  details?: Record<string, any>;
  timestamp: string;
  path: string;
}

export function getErrorMessage(error: unknown): string {
  if (error instanceof AxiosError && error.response?.data) {
    const apiError = error.response.data as ApiError;
    return apiError.message || 'An error occurred';
  }
  
  if (error instanceof Error) {
    return error.message;
  }
  
  return 'An unexpected error occurred';
}

export function getErrorDetails(error: unknown): ApiError | null {
  if (error instanceof AxiosError && error.response?.data) {
    return error.response.data as ApiError;
  }
  return null;
}

export function isSpecificErrorCode(error: unknown, code: string): boolean {
  const details = getErrorDetails(error);
  return details?.code === code;
}
```

### Error Boundary Component
```tsx
import React, { Component, ReactNode } from 'react';
import { Result, Button } from 'antd';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error?: Error;
}

export class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    console.error('Error caught by boundary:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      if (this.props.fallback) {
        return this.props.fallback;
      }

      return (
        <Result
          status="error"
          title="Something went wrong"
          subTitle="An unexpected error occurred. Please try refreshing the page."
          extra={
            <Button type="primary" onClick={() => window.location.reload()}>
              Refresh Page
            </Button>
          }
        />
      );
    }

    return this.props.children;
  }
}
```

## State Management Patterns

### Local State with useState
```tsx
import { useState, useEffect } from 'react';

export default function ComponentWithState() {
  const [loading, setLoading] = useState(false);
  const [data, setData] = useState<DataType[]>([]);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchData = async () => {
      setLoading(true);
      setError(null);
      
      try {
        const response = await api.get('/data');
        setData(response.data);
      } catch (err) {
        setError(getErrorMessage(err));
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, []);

  return (
    <div>
      {loading && <Spin />}
      {error && <Alert type="error" message={error} />}
      {data.map(item => <div key={item.id}>{item.name}</div>)}
    </div>
  );
}
```

### Custom Hooks Pattern
```tsx
import { useState, useCallback } from 'react';

export function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);

  const toggle = useCallback(() => setValue(v => !v), []);
  const setTrue = useCallback(() => setValue(true), []);
  const setFalse = useCallback(() => setValue(false), []);

  return { value, toggle, setTrue, setFalse };
}

export function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  const setValue = useCallback((value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  }, [key, storedValue]);

  return [storedValue, setValue] as const;
}
```

## Testing Patterns

### Component Testing
```tsx
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { vi } from 'vitest';
import ComponentName from './ComponentName';

const createTestQueryClient = () => new QueryClient({
  defaultOptions: {
    queries: { retry: false },
    mutations: { retry: false },
  },
});

function renderWithProviders(ui: React.ReactElement) {
  const queryClient = createTestQueryClient();
  
  return render(
    <QueryClientProvider client={queryClient}>
      {ui}
    </QueryClientProvider>
  );
}

describe('ComponentName', () => {
  it('should render with props', () => {
    renderWithProviders(<ComponentName prop="test" />);
    
    expect(screen.getByTestId('component-name')).toBeInTheDocument();
    expect(screen.getByText('test')).toBeInTheDocument();
  });

  it('should handle user interactions', async () => {
    const mockFn = vi.fn();
    renderWithProviders(<ComponentName onAction={mockFn} />);
    
    fireEvent.click(screen.getByRole('button'));
    
    await waitFor(() => {
      expect(mockFn).toHaveBeenCalled();
    });
  });
});
```

### Hook Testing
```tsx
import { renderHook, act } from '@testing-library/react';
import { useToggle } from './useToggle';

describe('useToggle', () => {
  it('should initialize with default value', () => {
    const { result } = renderHook(() => useToggle());
    
    expect(result.current.value).toBe(false);
  });

  it('should toggle value', () => {
    const { result } = renderHook(() => useToggle());
    
    act(() => {
      result.current.toggle();
    });
    
    expect(result.current.value).toBe(true);
  });
});
```

## Performance Optimization

### Memoization Patterns
```tsx
import React, { memo, useMemo, useCallback } from 'react';

interface ExpensiveComponentProps {
  data: DataType[];
  onItemClick: (id: string) => void;
}

const ExpensiveComponent = memo(function ExpensiveComponent({ 
  data, 
  onItemClick 
}: ExpensiveComponentProps) {
  const processedData = useMemo(() => {
    return data.map(item => ({
      ...item,
      processed: expensiveProcessing(item),
    }));
  }, [data]);

  const handleClick = useCallback((id: string) => {
    onItemClick(id);
  }, [onItemClick]);

  return (
    <div>
      {processedData.map(item => (
        <div key={item.id} onClick={() => handleClick(item.id)}>
          {item.processed}
        </div>
      ))}
    </div>
  );
});
```

### Lazy Loading
```tsx
import { lazy, Suspense } from 'react';
import { Spin } from 'antd';

const LazyComponent = lazy(() => import('./LazyComponent'));

export default function App() {
  return (
    <Suspense fallback={<Spin size="large" />}>
      <LazyComponent />
    </Suspense>
  );
}
```

## Styling Patterns

### CSS Modules
```tsx
import styles from './Component.module.css';

export default function Component() {
  return (
    <div className={styles.container}>
      <h1 className={styles.title}>Title</h1>
      <p className={styles.description}>Description</p>
    </div>
  );
}
```

### Conditional Classes
```tsx
import { clsx } from 'clsx';

interface ButtonProps {
  variant: 'primary' | 'secondary';
  size: 'small' | 'large';
  disabled?: boolean;
}

export default function Button({ variant, size, disabled }: ButtonProps) {
  return (
    <button 
      className={clsx(
        'btn',
        `btn-${variant}`,
        `btn-${size}`,
        { 'btn-disabled': disabled }
      )}
      disabled={disabled}
    >
      Click me
    </button>
  );
}
```