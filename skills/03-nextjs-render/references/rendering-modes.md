# Static vs Dynamic Rendering

## หลักการ

```
Static (default)  → render ตอน build time → เร็ว, ข้อมูลไม่ realtime
Dynamic           → render ทุก request    → ช้ากว่า, ข้อมูล fresh เสมอ
```

## วิธีบังคับ Dynamic

```ts
// วิธีที่ 1: fetch แบบ no-store
const res = await fetch(url, { cache: 'no-store' })

// วิธีที่ 2: ใช้ dynamic functions (auto-opt-in)
import { cookies, headers } from 'next/headers'
const cookieStore = cookies()   // → route กลายเป็น dynamic อัตโนมัติ

// วิธีที่ 3: ประกาศตรงๆ
export const dynamic = 'force-dynamic'
export const revalidate = 0      // เท่ากับ force-dynamic
```

## เลือกแบบไหน?

```
ข้อมูลเปลี่ยนแปลงบ่อย (stock, chat)   → dynamic + cache: 'no-store'
ข้อมูลเปลี่ยนเป็นรอบ (บทความ, สินค้า) → ISR + revalidate: 60
ข้อมูลคงที่ (about, landing page)      → static (default)
```
