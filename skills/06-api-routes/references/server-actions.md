# Server Actions มาตรฐาน (Frontend-only)

> Server Actions เรียก external backend API ผ่าน fetch() เท่านั้น
> ห้าม import db หรือ call database โดยตรง

## โครงสร้าง

```ts
// src/lib/actions/user.actions.ts
'use server'

import { z } from 'zod'
import { revalidatePath } from 'next/cache'
import { getSession } from '@lib/auth'

const BASE_URL = process.env.API_BASE_URL

const Schema = z.object({
  name:  z.string().min(1),
  email: z.string().email(),
})

export async function createUser(formData: FormData) {
  // 1. Auth
  const session = await getSession()
  if (!session) return { success: false, error: 'Unauthorized' }

  // 2. Validate input ฝั่ง frontend
  const parsed = Schema.safeParse({
    name:  formData.get('name'),
    email: formData.get('email'),
  })
  if (!parsed.success) {
    return { success: false, error: parsed.error.issues[0].message }
  }

  // 3. เรียก external backend API (ไม่ใช่ database โดยตรง)
  try {
    const res = await fetch(`${BASE_URL}/users`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${session.accessToken}`,
      },
      body: JSON.stringify(parsed.data),
    })

    if (!res.ok) {
      const err = await res.json().catch(() => ({}))
      return { success: false, error: err.message ?? 'Request failed' }
    }
  } catch {
    return { success: false, error: 'Failed to create user' }
  }

  // 4. Revalidate + return
  revalidatePath('/users')
  return { success: true }
}
```

## การใช้งานใน Client Component

```tsx
'use client'
import { createUser } from '@lib/actions/user.actions'
import { useTransition } from 'react'

function CreateUserForm() {
  const [isPending, startTransition] = useTransition()

  const handleSubmit = (formData: FormData) => {
    startTransition(async () => {
      const result = await createUser(formData)
      if (!result.success) alert(result.error)
    })
  }

  return (
    <form action={handleSubmit}>
      <input name="name" required />
      <input name="email" type="email" required />
      <button disabled={isPending}>
        {isPending ? 'Creating...' : 'Create'}
      </button>
    </form>
  )
}
```
