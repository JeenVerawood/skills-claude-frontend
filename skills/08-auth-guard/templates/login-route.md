# Template: Login/Logout Route (copy ได้เลย)

> Login Route ทำหน้าที่รับ credentials จาก client → ส่งไป external backend API
> → รับ token กลับมา → set httpOnly cookie
> ห้าม verify credentials โดยตรงในไฟล์นี้ — นั่นคือ backend business logic

```ts
// app/api/auth/login/route.ts
import { NextResponse } from 'next/server'
import { z } from 'zod'

const BASE_URL = process.env.API_BASE_URL

const LoginSchema = z.object({
  email:    z.string().email(),
  password: z.string().min(1),
})

export async function POST(req: Request) {
  const parsed = LoginSchema.safeParse(await req.json())
  if (!parsed.success) {
    return NextResponse.json({ error: 'Invalid input' }, { status: 400 })
  }

  // ส่งไป backend เพื่อ verify credentials — ไม่ทำเองใน Next.js
  const authRes = await fetch(`${BASE_URL}/auth/login`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(parsed.data),
  }).catch(() => null)

  if (!authRes || !authRes.ok) {
    return NextResponse.json({ error: 'Invalid credentials' }, { status: 401 })
  }

  const { accessToken, user } = await authRes.json()

  const response = NextResponse.json({ success: true, user })

  response.cookies.set('auth-token', accessToken, {
    httpOnly: true,
    secure:   process.env.NODE_ENV === 'production',
    sameSite: 'lax',
    maxAge:   60 * 60 * 24 * 7,
    path:     '/',
  })

  return response
}
```

```ts
// app/api/auth/logout/route.ts
import { NextResponse } from 'next/server'

export async function DELETE() {
  const response = NextResponse.json({ success: true })
  response.cookies.delete('auth-token')
  return response
}
```
