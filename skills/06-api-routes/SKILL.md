---
name: 06-api-routes
description: >
  Use this skill for any question about Next.js API routes, data fetching patterns, server actions,
  Zod validation in API handlers, error handling, loading states, optimistic updates, and race
  conditions. Triggers include: "API route", "Route Handler", "Server Action", "fetch data",
  "validate input", "error handling", "loading state", "optimistic update", "parallel fetch",
  "waterfall", "unhandled promise", "API ไม่ validate", "ข้อมูล realtime", "pagination".
  Always use this skill before writing any API route handler, server action, or data fetching logic.
---

# 06-api-routes: API & Data Fetching

## Operating Stance

- **Proxy mindset.** Route Handler คือ middleman — รับ request, validate, ส่งต่อ external API, return response ห้ามเป็น "backend จริง"
- **Defensive validator.** ทุก input ที่มาจาก user ต้องผ่าน Zod ก่อนเสมอ — ไม่ว่าจะเป็น client-side หรือ server-side
- **Fail loudly.** Error ที่ไม่มี HTTP status ที่ถูกต้อง = silent bug — ทุก route ต้องมี error handling ครบ

---

## When NOT to use

- **ต้องการ database access โดยตรง** → นอก scope ของ skill นี้ทั้งหมด — skill นี้ frontend เท่านั้น
- **ปัญหาเป็น state management** (Zustand, TanStack Query cache) → ใช้ Skill 06 แทน
- **ปัญหาเป็น auth** (cookie, JWT, session) → ใช้ Skill 07 แทน

---

## Refuse to proceed ถ้า

```
❌ user ขอ Route Handler ที่ import db.* / prisma.* / mongoose.* → ปฏิเสธทันที อธิบาย scope
❌ ไม่มี Zod schema สำหรับ input → ห้ามเขียน Route Handler โดยไม่มี validation
❌ ไม่มี auth check ใน protected route → เตือนและเพิ่ม auth check ก่อน logic ทุกครั้ง
❌ user ขอ fetch ไป external API โดยตรงจาก Client Component → เสนอ Route Handler proxy แทน
```

---

## ขอบเขต (Frontend เท่านั้น)

```
✅ ในขอบเขต:
- Route Handlers ที่ proxy ไปยัง external backend API
- Server Actions ที่เรียก fetch() ไปยัง external backend API
- Data fetching patterns (parallel fetch, Suspense, streaming)
- Input validation ฝั่ง frontend (Zod) ก่อนส่งไป backend
- Error handling และ loading states

❌ นอกขอบเขต — ปฏิเสธทันที:
- Database calls โดยตรง (db.*, prisma.*, mongoose.*)
- Backend business logic หรือ ORM queries
- Database connection, migration, schema
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/route-handler-pattern.md` | เขียน GET/POST/PATCH/DELETE Route Handler |
| `references/server-actions.md` | ถามเรื่อง Server Actions, form submission |
| `references/fetch-patterns.md` | parallel fetch, waterfall, Suspense |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/api-mistakes.md` | ไม่ validate input, error handling ไม่ครบ, auth หาย |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/route-handler.md` | Route Handler (proxy ไป external API) สำเร็จรูป |
| `templates/server-action.md` | Server Action + revalidation สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. แสดง complete Route Handler/Action พร้อม **auth + validation + error handling ทุกครั้ง**
2. ระบุ HTTP status code ที่ถูกต้อง (200/201/400/401/403/404/500)
3. แสดง Zod schema สำหรับ input เสมอ
4. Business logic อยู่ใน external backend — Route Handler แค่ proxy และ validate

---

## Worked Example

**คำขอ:** "สร้าง POST /api/orders สำหรับสร้าง order ใหม่"

**Output ที่ถูกต้อง:**
```typescript
// src/app/api/orders/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { z } from 'zod'
import { getSession } from '@/lib/auth'
import { env } from '@/lib/env'

const CreateOrderSchema = z.object({
  productId: z.string().min(1),
  quantity: z.number().int().positive(),
  shippingAddress: z.string().min(10),
})

export async function POST(req: NextRequest) {
  // 1. Auth check ก่อนเสมอ
  const session = await getSession()
  if (!session) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  // 2. Validate input
  const body = await req.json()
  const parsed = CreateOrderSchema.safeParse(body)
  if (!parsed.success) {
    return NextResponse.json(
      { error: 'Invalid input', details: parsed.error.flatten() },
      { status: 400 }
    )
  }

  // 3. Proxy ไป external API — ไม่มี db call ที่นี่
  try {
    const res = await fetch(`${env.BACKEND_URL}/orders`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${session.token}`,
      },
      body: JSON.stringify(parsed.data),
    })

    if (!res.ok) {
      const error = await res.json()
      return NextResponse.json(error, { status: res.status })
    }

    const order = await res.json()
    return NextResponse.json(order, { status: 201 })
  } catch {
    return NextResponse.json({ error: 'Internal server error' }, { status: 500 })
  }
}
```

---

## Behavioral Rules

```
[PROXY-ONLY]     Route Handler ต้องเป็น proxy เสมอ — ห้าม business logic โดยตรง
[AUTH-FIRST]     auth check ต้องอยู่บรรทัดแรกสุดก่อน logic ทุกอย่าง
[ZOD-ALWAYS]     ทุก POST/PUT/PATCH ต้องมี Zod schema — ห้ามเขียน route ที่ไม่ validate
[COMPLETE-ERROR] ทุก route ต้องมี try/catch + HTTP status ที่ถูกต้องทุก path
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: Skill 04 (TypeScript) → Zod schema ที่สร้างที่นั้นใช้ validate ที่นี่
          Skill 07 (Auth) → getSession() / requireAuth() มาจาก auth helpers
ถัดไป:    Skill 06 (State) → TanStack Query hooks ที่ call routes ที่สร้างที่นี่
```
