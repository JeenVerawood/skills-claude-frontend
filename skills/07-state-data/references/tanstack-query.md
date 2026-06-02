# TanStack Query

## Setup

```tsx
// app/providers.tsx
'use client'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { useState } from 'react'

export function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(() => new QueryClient({
    defaultOptions: { queries: { staleTime: 1000 * 60, retry: 2 } },
  }))
  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  )
}
```

## Query Keys Convention

```ts
// src/lib/query-keys.ts
export const queryKeys = {
  users: {
    all:    ['users'] as const,
    lists:  () => [...queryKeys.users.all, 'list'] as const,
    list:   (f: Record<string, unknown>) => [...queryKeys.users.lists(), f] as const,
    detail: (id: string) => [...queryKeys.users.all, 'detail', id] as const,
  },
}
```

## Custom Hook Pattern

```ts
// hooks/useUsers.ts
export function useUsers(filters?: Record<string, unknown>) {
  return useQuery({
    queryKey: queryKeys.users.list(filters ?? {}),
    queryFn: async () => {
      const res = await fetch(`/api/users?${new URLSearchParams(filters as any)}`)
      if (!res.ok) throw new Error('Failed to fetch')
      return res.json() as Promise<{ data: User[] }>
    },
  })
}

export function useCreateUser() {
  const qc = useQueryClient()
  return useMutation({
    mutationFn: (input: CreateUserInput) =>
      fetch('/api/users', { method: 'POST', body: JSON.stringify(input) }).then(r => r.json()),
    onSuccess: () => qc.invalidateQueries({ queryKey: queryKeys.users.all }),
  })
}
```
