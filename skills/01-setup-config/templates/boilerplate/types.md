# Type Files มาตรฐาน (copy ได้เลย)

## src/types/index.ts

```ts
// Re-export ทุก type จากที่นี่ที่เดียว
// การใช้งาน: import type { User } from '@/types'

// export * from './user.types'
// export * from './api.types'
// export * from './common.types'

// Common utility types — ใช้ได้เลย ไม่ต้อง define ใหม่
export type Result<T, E = string> =
  | { success: true; data: T }
  | { success: false; error: E }

export type Nullable<T> = T | null
export type Optional<T> = T | undefined
export type AsyncFn<T = void> = () => Promise<T>
```

---

## src/types/api.types.ts

```ts
// Response types มาตรฐาน — ทุก API ต้องใช้ format นี้
export type ApiSuccess<T> = {
  data: T
  message?: string
}

export type ApiError = {
  error: string
  details?: unknown
  code?: string
}

export type PaginatedResponse<T> = {
  data: T[]
  total: number
  page: number
  pageSize: number
  hasNext: boolean
  nextCursor?: string
}
```

---

## src/types/common.types.ts

```ts
// Shared types ที่ใช้ทั่ว project
export type Status = 'idle' | 'loading' | 'success' | 'error'

export type SortOrder = 'asc' | 'desc'

export type PaginationParams = {
  page?: number
  limit?: number
  cursor?: string
}

export type WithTimestamps = {
  createdAt: Date
  updatedAt: Date
}

export type WithId = {
  id: string
}

export type BaseEntity = WithId & WithTimestamps
```
