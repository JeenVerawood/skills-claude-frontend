# Template: Auth Helpers (copy ได้เลย)

```ts
// src/lib/auth.ts
import { cookies } from 'next/headers'
import { verifyJWT } from './jwt'
import { NextResponse } from 'next/server'

export type Session = {
  userId: string
  email: string
  role: 'admin' | 'user'
  accessToken: string
}

// ใช้ใน Server Components และ Route Handlers
export async function getSession(): Promise<Session | null> {
  const cookieStore = cookies()
  const token = cookieStore.get('auth-token')?.value
  if (!token) return null
  try {
    return await verifyJWT(token)
  } catch {
    return null
  }
}

// throw ถ้าไม่มี session
export async function requireAuth(): Promise<Session> {
  const session = await getSession()
  if (!session) throw new Error('Unauthorized')
  return session
}

// throw ถ้าไม่มีสิทธิ์
export async function requireRole(role: 'admin'): Promise<Session> {
  const session = await requireAuth()
  if (session.role !== role) throw new Error('Forbidden')
  return session
}

// Helper สำหรับ Route Handlers
export function unauthorized() {
  return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
}

export function forbidden() {
  return NextResponse.json({ error: 'Forbidden' }, { status: 403 })
}
```

## การใช้งานใน Route Handler (proxy ไป external API)

```ts
// app/api/users/[id]/route.ts
export async function DELETE(_req: Request, { params }: { params: { id: string } }) {
  try {
    await requireRole('admin')
    const session = await getSession()

    // proxy ไป external backend — ไม่เรียก db โดยตรง
    const res = await fetch(`${process.env.API_BASE_URL}/users/${params.id}`, {
      method: 'DELETE',
      headers: { Authorization: `Bearer ${session!.accessToken}` },
    })

    if (!res.ok) {
      const err = await res.json().catch(() => ({}))
      return NextResponse.json(err, { status: res.status })
    }

    return NextResponse.json({ success: true })
  } catch (err) {
    if (err instanceof Error) {
      if (err.message === 'Unauthorized') return unauthorized()
      if (err.message === 'Forbidden') return forbidden()
    }
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
  }
}
```
