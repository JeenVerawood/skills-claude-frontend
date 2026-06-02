# Template: Result Pattern — Type-safe Async

```ts
// src/types/common.types.ts
export type Result<T, E = string> =
  | { success: true; data: T }
  | { success: false; error: E }

// ใช้งาน — async function ที่ระบุ return type ครบ
import { z } from 'zod'
import { UserSchema } from '@types/user.types'
import type { Result } from '@types/common.types'

export const fetchUser = async (id: string): Promise<Result<User>> => {
  try {
    const res = await fetch(`/api/user/${id}`)
    if (!res.ok) return { success: false, error: `HTTP ${res.status}` }

    const raw = await res.json()
    const data = UserSchema.parse(raw)   // Zod validate runtime
    return { success: true, data }
  } catch (err) {
    return {
      success: false,
      error: err instanceof Error ? err.message : 'Unknown error',
    }
  }
}

// การใช้งาน — TypeScript บังคับให้เช็ค success ก่อนใช้ data
const result = await fetchUser('123')
if (result.success) {
  console.log(result.data.name)   // TypeScript รู้ว่า data คือ User
} else {
  console.error(result.error)     // TypeScript รู้ว่า error คือ string
}
```
