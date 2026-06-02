# Auth Layers — ต้องเช็คทุกชั้น

## 4 ชั้นที่ต้องครอบคลุม

```
ชั้น 1: Middleware        → ป้องกัน route ที่ไม่ได้รับอนุญาต
ชั้น 2: API Route Guard   → เช็ค session ทุก endpoint
ชั้น 3: Server Component  → เช็คก่อน render ข้อมูล sensitive
ชั้น 4: Client UI         → ซ่อน UI (UX เท่านั้น ไม่ใช่ security)

กฎ: ต้องมีชั้น 1 + 2 เสมอ — ขาดชั้นใดชั้นหนึ่งคือช่องโหว่
```

## ชั้น 3: Server Component

```tsx
// app/dashboard/page.tsx
import { redirect } from 'next/navigation'
import { getSession } from '@lib/auth'

export default async function DashboardPage() {
  const session = await getSession()
  if (!session) redirect('/login')   // server-side redirect

  const data = await fetchSensitiveData(session.userId)
  return <Dashboard data={data} />
}
```

→ Middleware template: `../../03-nextjs-render/templates/middleware-auth.md`
→ API route template: `../../08-auth-guard/templates/auth-helpers.md`
