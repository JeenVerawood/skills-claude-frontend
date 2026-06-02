# Template: Server Action (copy ได้เลย)

> Server Action เรียก external backend API ผ่าน fetch() เท่านั้น
> ห้าม import db หรือเรียก database โดยตรง

```ts
// src/lib/actions/[resource].actions.ts
'use server'

import { z } from 'zod'
import { revalidatePath, revalidateTag } from 'next/cache'
import { getSession } from '@lib/auth'

const BASE_URL = process.env.API_BASE_URL

// Schema
const CreateSchema = z.object({
  name:  z.string().min(1, 'Name is required').max(100),
  email: z.string().email('Invalid email'),
})

type ActionResult = { success: true } | { success: false; error: string }

// Create
export async function createUser(formData: FormData): Promise<ActionResult> {
  const session = await getSession()
  if (!session) return { success: false, error: 'Unauthorized' }

  const parsed = CreateSchema.safeParse({
    name:  formData.get('name'),
    email: formData.get('email'),
  })
  if (!parsed.success) return { success: false, error: parsed.error.issues[0].message }

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
      return { success: false, error: err.message ?? 'Failed to create user' }
    }

    revalidateTag('users')
    revalidatePath('/users')
    return { success: true }
  } catch {
    return { success: false, error: 'Failed to create user' }
  }
}

// Delete
export async function deleteUser(id: string): Promise<ActionResult> {
  const session = await getSession()
  if (!session) return { success: false, error: 'Unauthorized' }
  if (session.role !== 'admin') return { success: false, error: 'Forbidden' }

  try {
    const res = await fetch(`${BASE_URL}/users/${id}`, {
      method: 'DELETE',
      headers: { Authorization: `Bearer ${session.accessToken}` },
    })

    if (!res.ok) {
      const err = await res.json().catch(() => ({}))
      return { success: false, error: err.message ?? 'Failed to delete user' }
    }

    revalidateTag('users')
    return { success: true }
  } catch {
    return { success: false, error: 'Failed to delete user' }
  }
}
```
