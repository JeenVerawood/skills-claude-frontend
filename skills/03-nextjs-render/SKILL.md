---
name: 03-nextjs-render
description: >
  Use this skill for any question about Next.js App Router internals: caching behavior,
  revalidation, hydration mismatch, static vs dynamic rendering, Middleware configuration,
  loading/error/not-found files, nested layouts, next/image, next/font, Parallel Routes,
  Intercepting Routes, generateStaticParams, build/dev performance, and Turbopack issues.
  Triggers include: "cache ไม่ update", "hydration error", "ข้อมูลเก่า", "middleware",
  "revalidatePath", "revalidateTag", "static rendering", "dynamic rendering", "next/image",
  "loading.tsx", "error.tsx", "generateStaticParams", "parallel routes". Always use this skill
  before writing any Next.js route, middleware, or fetch configuration code.
---

# Skill 03: 03-nextjs-render

## Operating Stance

- **Trace ก่อน fix.** ก่อนแนะนำ solution ให้ trace request lifecycle ก่อนว่า request ไปถึงไหน cache ชั้นไหนกักอยู่
- **Layer-aware.** Next.js มี cache หลายชั้น — ระบุให้ได้ว่าปัญหาอยู่ชั้นไหนก่อนแก้
- **Server-first.** Default คือ Server Component เสมอ — `'use client'` ต้องมีเหตุผลที่ justifiable

---

## When NOT to use

- **ปัญหาเป็น React logic** (useState, useEffect loop) → ใช้ Skill 03 แทน
- **ปัญหาเป็น TypeScript** → ใช้ Skill 04 แทน
- **ปัญหาเป็น API/fetch pattern** → ใช้ Skill 05 แทน

---

## Refuse to proceed ถ้า

```
❌ user บอกว่า "cache ไม่ทำงาน" โดยไม่บอก route หรือ fetch URL → ถามก่อน
❌ user ขอ revalidate แต่ไม่รู้ว่า route เป็น static หรือ dynamic → ถามก่อน
❌ user ขอใส่ 'use client' บน layout.tsx หรือ page.tsx ทั้งหมด → ห้าม comply อธิบายผลกระทบก่อน
```

---

## วิธีใช้ไฟล์ใน Skill นี้

**view ไฟล์ที่ตรงกับ topic ก่อนตอบทุกครั้ง** — ห้ามตอบจากความจำ

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/caching-layers.md` | ถามเรื่อง cache, revalidate, ข้อมูลเก่า |
| `references/rendering-modes.md` | ถามเรื่อง static vs dynamic, ISR |
| `references/route-conventions.md` | ถามเรื่อง loading.tsx, error.tsx, not-found.tsx, layout |
| `references/middleware.md` | ถามเรื่อง middleware, route protection, matcher |
| `references/next-image-font.md` | ถามเรื่อง next/image, next/font, layout shift |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/cache-mistakes.md` | cache ไม่ทำงาน, ข้อมูลเก่าค้าง |
| `bad-patterns/hydration-mistakes.md` | hydration mismatch, browser API บน server |
| `bad-patterns/route-mistakes.md` | dynamic slug conflict, searchParams ผิด, route error |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/middleware-auth.md` | scaffold middleware สำหรับ auth |
| `templates/error-boundary.md` | error.tsx, not-found.tsx สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. **ระบุ layer** ที่ปัญหาอยู่ (Request Memoization / Data Cache / Full Route Cache / Router Cache)
2. แสดง ❌ จาก `bad-patterns/` พร้อมอธิบายว่าทำไมถึงเกิดปัญหา
3. แสดง ✅ solution จาก `references/` พร้อม code
4. Warning edge cases ถ้ามี (เช่น revalidate กับ streaming ใช้ด้วยกันไม่ได้)

---

## Worked Example

**ปัญหา:** "fetch ข้อมูลแล้ว cache อยู่ ไม่ update เมื่อ user สั่ง submit form"

**วิธีตอบที่ถูกต้อง:**
```
Layer ที่ปัญหาอยู่: Data Cache (Next.js cache ผล fetch ไว้)

❌ Pattern ผิด:
async function getProducts() {
  const res = await fetch('/api/products') // cached ตลอดไป
  return res.json()
}

✅ Solution: ใช้ revalidateTag หรือ no-store
// ตัวเลือก 1 — tag-based revalidation
const res = await fetch('/api/products', {
  next: { tags: ['products'] }
})
// ใน Server Action: revalidateTag('products')

// ตัวเลือก 2 — dynamic route ไม่ cache เลย
const res = await fetch('/api/products', {
  cache: 'no-store'
})

เลือก tag-based ถ้า: ต้องการ revalidate จาก action เฉพาะจุด
เลือก no-store ถ้า: ข้อมูล realtime ที่ต้องใหม่ทุก request
```

---

## Behavioral Rules

```
[LAYER-FIRST]      ระบุ cache layer ก่อนเสมอ — ห้ามเสนอ solution โดยไม่รู้ว่าปัญหาอยู่ชั้นไหน
[NO-USE-CLIENT]    ห้าม recommend 'use client' บน layout/page ทั้งหมด — ชี้ให้ถูก component แทน
[SHOW-TRADEOFF]    ทุก solution ที่มีหลายตัวเลือก → บอก tradeoff ของแต่ละตัวด้วย
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append Bug Fix entry ลง LOG.md ทันที
                   format ต้องมี: trigger (error message), root cause, files modified, fix
```

## Cross-skill Chain

```
ก่อนหน้า: start-all-project Phase 5 → ใช้ references/rendering-modes.md กำหนด strategy
ถัดไป:    Skill 03 (React Patterns) → หลังกำหนด rendering แล้ว audit component patterns
          Skill 05 (API) → ถ้าปัญหาเกี่ยวกับ fetch ใน Route Handler
```
