# Fetch Patterns

## Parallel Fetch — ลด waterfall

```ts
// ❌ Sequential — 200+200 = 400ms
const user  = await fetchUser(id)
const posts = await fetchPosts(id)

// ✅ Parallel — max(200,200) = 200ms
const [user, posts] = await Promise.all([fetchUser(id), fetchPosts(id)])

// ✅ Parallel + handle error แยกกัน
const [userResult, postsResult] = await Promise.allSettled([
  fetchUser(id),
  fetchPosts(id),
])
const user  = userResult.status  === 'fulfilled' ? userResult.value  : null
const posts = postsResult.status === 'fulfilled' ? postsResult.value : []
```

## Server Component Fetch

```tsx
// ✅ fetch ใน Server Component — ไม่ต้อง useEffect
export default async function DashboardPage() {
  const [stats, orders] = await Promise.all([
    fetchDashboardStats(),
    fetchRecentOrders({ limit: 10 }),
  ])
  return (
    <main>
      <StatsSection stats={stats} />
      <OrdersTable orders={orders} />
    </main>
  )
}
```

## Suspense + Streaming

```tsx
import { Suspense } from 'react'

export default function Page() {
  return (
    <div>
      <Suspense fallback={<StatsSkeleton />}>
        <SlowStatsSection />   {/* fetch ช้า — stream แยก */}
      </Suspense>
      <Suspense fallback={<TableSkeleton />}>
        <SlowTableSection />
      </Suspense>
    </div>
  )
}
```

## Cursor Pagination — fetch จาก external API

```ts
// src/lib/api/posts.ts
const BASE_URL = process.env.API_BASE_URL

export async function fetchPosts(cursor?: string, limit = 20) {
  const params = new URLSearchParams({ limit: String(limit) })
  if (cursor) params.set('cursor', cursor)

  const res = await fetch(`${BASE_URL}/posts?${params}`, {
    cache: 'no-store',
  })

  if (!res.ok) throw new Error('Failed to fetch posts')
  return res.json() as Promise<{ data: Post[]; nextCursor?: string }>
}
```

## API Helper มาตรฐาน

```ts
// src/lib/api/client.ts
const BASE_URL = process.env.API_BASE_URL

export async function apiFetch<T>(
  path: string,
  options?: RequestInit & { token?: string }
): Promise<T> {
  const { token, ...init } = options ?? {}
  const res = await fetch(`${BASE_URL}${path}`, {
    ...init,
    headers: {
      'Content-Type': 'application/json',
      ...(token && { Authorization: `Bearer ${token}` }),
      ...init.headers,
    },
  })

  if (!res.ok) {
    const err = await res.json().catch(() => ({}))
    throw Object.assign(new Error(err.message ?? 'Request failed'), { status: res.status })
  }

  return res.json()
}
```
