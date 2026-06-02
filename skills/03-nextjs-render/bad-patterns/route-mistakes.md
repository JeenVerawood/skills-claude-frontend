# Route Anti-patterns

## ❌ Mistake 1: Dynamic Slug ชนกันในระดับเดียวกัน

```
Error: You cannot use different slug names for the same dynamic path
('restaurantId' !== 'queueId')
```

```
/* ❌ slug ต่างชื่อกันในระดับเดียวกัน */
app/api/queues/[queueId]/route.ts
app/api/queues/[restaurantId]/stream/route.ts
                ↑                ↑
          คนละชื่อ แต่อยู่ระดับเดียวกัน → ERROR
```

**ทำไมผิด:** Next.js App Router มองว่า `/api/queues/[xxx]` เป็น path template เดียวกัน
param name ต้องตรงกันทุก route ที่อยู่ใต้ path เดียวกัน

```
/* ✅ ใช้ชื่อเดียวกันทั้งหมดในระดับนั้น */
app/api/queues/[id]/route.ts           ← params.id = queueId
app/api/queues/[id]/stream/route.ts    ← params.id = restaurantId หรือ queueId
```

**กฎ:** ก่อนสร้าง API routes ให้วาด tree ก่อนเสมอ — ถ้า `[xxx]` อยู่ระดับเดียวกัน
ต้องใช้ชื่อเดียวกัน แนะนำใช้ `[id]` เป็น default ถ้าไม่แน่ใจ

---

## ❌ Mistake 2: สร้าง dynamic route ซ้อนกันโดยไม่วางแผน

```
/* ❌ ตัวอย่าง path ที่ slug ชนกัน — error ทันที */
app/
├── api/
│   ├── posts/[postId]/route.ts
│   └── posts/[slug]/comments/route.ts     ← ชนกับ [postId]!
│
├── users/[userId]/route.ts
└── users/[id]/orders/route.ts             ← ชนกับ [userId]!
```

```
/* ✅ ตัวอย่าง path ที่ถูกต้อง */
app/
├── api/
│   └── posts/[id]/route.ts                ← id ใช้ทั้ง postId และ slug
│   └── posts/[id]/comments/route.ts       ← [id] ชื่อเดียวกัน ✓
│
└── users/[id]/route.ts
    └── users/[id]/orders/route.ts         ← [id] ชื่อเดียวกัน ✓
```

---

## ❌ Mistake 3: ลืมเพิ่ม `export const dynamic = 'force-dynamic'` ใน page ที่ใช้ searchParams

```tsx
/* ❌ page ที่ใช้ searchParams แต่ไม่ประกาศ dynamic */
export default function SearchPage({
  searchParams,
}: {
  searchParams: { q?: string }
}) {
  // Next.js อาจ cache หน้านี้แบบ static → searchParams ไม่ update
  return <Results query={searchParams.q} />
}
```

```tsx
/* ✅ ประกาศ dynamic ทุกครั้งที่ใช้ searchParams */
export const dynamic = 'force-dynamic'

export default function SearchPage({ searchParams }: { searchParams: { q?: string } }) {
  return <Results query={searchParams.q} />
}
```

---

## ❌ Mistake 4: ใช้ `useSearchParams()` ใน Server Component

```tsx
/* ❌ useSearchParams เป็น hook — ใช้ได้เฉพาะ Client Component */
import { useSearchParams } from 'next/navigation'

export default function Page() {        // ← ไม่มี 'use client'
  const params = useSearchParams()      // ← Error!
  return <div>{params.get('q')}</div>
}
```

```tsx
/* ✅ วิธีที่ 1: ใช้ searchParams prop ใน Server Component */
export default function Page({ searchParams }: { searchParams: { q?: string } }) {
  return <div>{searchParams.q}</div>
}

/* ✅ วิธีที่ 2: แยก Client Component สำหรับส่วนที่ต้องใช้ useSearchParams */
'use client'
import { useSearchParams } from 'next/navigation'

export function SearchInput() {
  const params = useSearchParams()
  return <input defaultValue={params.get('q') ?? ''} />
}
```
