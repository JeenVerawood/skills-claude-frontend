# Next.js Caching Layers

## 4 ชั้น Cache (ต้องรู้ทั้งหมด)

```
ชั้น 1: Request Memoization
  scope   → ใน 1 render cycle เดียวกัน
  reset   → ทุก request อัตโนมัติ
  ใช้เมื่อ → dedup fetch calls ใน component tree เดียวกัน

ชั้น 2: Data Cache
  scope   → ข้าม request (server-side, persistent)
  reset   → revalidatePath / revalidateTag / time-based
  ใช้เมื่อ → fetch() ทั่วไป (default พฤติกรรม)

ชั้น 3: Full Route Cache
  scope   → static HTML + RSC payload ของ page
  reset   → redeploy หรือ revalidate
  ใช้เมื่อ → static page ที่ build ครั้งเดียว

ชั้น 4: Router Cache (Client)
  scope   → browser memory ระหว่าง navigation
  reset   → hard refresh หรือ session หมด
  ใช้เมื่อ → navigation ระหว่าง page บน client
```

## Cache Control Cheatsheet

```ts
// Realtime — ไม่ cache เลย
fetch(url, { cache: 'no-store' })

// Revalidate ทุก 60 วินาที
fetch(url, { next: { revalidate: 60 } })

// Tag-based — revalidate เมื่อมีการเปลี่ยนแปลง
fetch(url, { next: { tags: ['products'] } })

// On-demand revalidation (Server Action / Route Handler)
import { revalidatePath, revalidateTag } from 'next/cache'
revalidatePath('/dashboard')      // revalidate path
revalidateTag('products')         // revalidate ทุก fetch ที่ใช้ tag นี้
```

## Route-level Cache Control

```ts
// app/dashboard/page.tsx
export const revalidate = 60         // ISR — revalidate ทุก 60 วินาที
export const dynamic = 'force-dynamic'  // dynamic ทุก request
export const revalidate = 0          // = force-dynamic
```
