# API Anti-patterns

## ❌ Mistake 1: ไม่ validate input

```ts
export async function POST(req: Request) {
  const body = await req.json()
  await createUser(body.name, body.email)  // 💥 ถ้า field หาย หรือ type ผิด
}
```

✅ Fix: Zod safeParse ทุก endpoint (ดู `templates/route-handler.md`)

---

## ❌ Mistake 2: ไม่เช็ค Auth ใน API route

```ts
// ❌ ใครก็เรียก API ได้โดยไม่ต้อง login!
export async function DELETE(req: Request, { params }) {
  const res = await fetch(`${BASE_URL}/users/${params.id}`, { method: 'DELETE' })
  return Response.json({ success: true })
}
```

✅ Fix: เช็ค session บรรทัดแรกเสมอ

```ts
const session = await getSession()
if (!session) return Response.json({ error: 'Unauthorized' }, { status: 401 })
```

---

## ❌ Mistake 3: Error handling ไม่ครบ

```ts
export async function GET() {
  const res = await fetch(`${BASE_URL}/users`)  // 💥 ถ้า backend ล่ม → unhandled rejection
  return Response.json(await res.json())
}
```

✅ Fix: try/catch ทุก handler + log error

```ts
try {
  const res = await fetch(`${BASE_URL}/users`, { cache: 'no-store' })
  if (!res.ok) {
    return Response.json({ error: 'Upstream error' }, { status: res.status })
  }
  return Response.json(await res.json())
} catch (err) {
  console.error('[GET /api/users]', err)
  return Response.json({ error: 'Internal server error' }, { status: 500 })
}
```

---

## ❌ Mistake 4: Sequential fetch (waterfall)

```ts
// ❌ 200ms + 200ms = 400ms
const user  = await fetchUser(id)
const posts = await fetchPosts(id)
```

✅ Fix: Promise.all

```ts
// ✅ max(200ms, 200ms) = 200ms
const [user, posts] = await Promise.all([fetchUser(id), fetchPosts(id)])
```

---

## ❌ Mistake 5: HTTP status code ผิด

```ts
// ❌ ใช้ 200 ทุกกรณีรวมถึง error
return Response.json({ error: 'Not found' })  // status 200 แต่มี error!
```

✅ Status Code Convention
```
200 → GET/PATCH/DELETE สำเร็จ
201 → POST สำเร็จ (สร้างใหม่)
400 → Validation failed
401 → ไม่มี session
403 → มี session แต่ไม่มีสิทธิ์
404 → ไม่พบ resource
409 → Conflict (email ซ้ำ)
500 → Internal server error
```

---

## ❌ Mistake 6: เรียก database โดยตรงใน Route Handler

```ts
// ❌ Route Handler ใน Next.js ไม่ใช่ backend — ห้าม import db
import { db } from '@lib/db'

export async function GET() {
  const users = await db.user.findMany()  // ❌ นี่คือ backend code
  return Response.json(users)
}
```

✅ Fix: Route Handler ทำหน้าที่ proxy ไป external backend API เท่านั้น

```ts
export async function GET() {
  const session = await getSession()
  if (!session) return Response.json({ error: 'Unauthorized' }, { status: 401 })

  const res = await fetch(`${process.env.API_BASE_URL}/users`, {
    headers: { Authorization: `Bearer ${session.accessToken}` },
    cache: 'no-store',
  })
  return Response.json(await res.json(), { status: res.status })
}
```
