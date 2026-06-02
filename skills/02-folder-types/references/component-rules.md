# Component Classification Rules

## แยก Client vs Server Component ด้วย suffix

> Project นี้ใช้ **suffix pattern** แทน folder แยก — ดูที่ชื่อไฟล์

| Pattern | ตำแหน่ง | ลักษณะ |
|---|---|---|
| `*Client.tsx` | `components/` | มี `'use client'`, ใช้ useState/useEffect/browser API |
| `*.tsx` (ไม่มี suffix) | `components/` | Server-safe, รับ props แสดงผลอย่างเดียว |

## ตัวอย่างจาก project จริง

```
components/
├── DashboardClient.tsx   ← 'use client', useState(activeFloor)
├── RoomsClient.tsx       ← 'use client', useState, fetch API
├── BillsClient.tsx       ← 'use client', useState, useCallback
├── MeterClient.tsx       ← 'use client', useState(floorFilter)
├── ConfigClient.tsx      ← 'use client', useState, fetch API
│
├── StatCard.tsx          ← Server-safe, pure display
├── RoomCard.tsx          ← Server-safe, Framer Motion + props
├── BillTable.tsx         ← 'use client' (motion), รับ callback props
├── MeterForm.tsx         ← 'use client', useCallback(updateRow)
├── OccupancyRing.tsx     ← 'use client' (motion)
├── RevenueBar.tsx        ← 'use client' (motion)
├── Sidebar.tsx           ← 'use client' (usePathname)
└── Topbar.tsx            ← 'use client' (useTheme, signOut)
```

## กฎการตัดสิน — ต้องการ 'use client' เมื่อไหร่

```
ใช้ useState / useReducer / useEffect   → 'use client'
ใช้ useRouter / usePathname             → 'use client'
ใช้ browser API (window, document)      → 'use client'
ใช้ Framer Motion (motion.*)            → 'use client'
ใช้ next-themes / next-auth/react        → 'use client'

แค่รับ props และ render                → ไม่ต้อง 'use client'
ดึงข้อมูลจาก DB / mockData             → ไม่ต้อง 'use client' (Server Component)
```

กฎ: `'use client'` ต้องอยู่ในไฟล์ leaf component เล็กๆ เท่านั้น
ห้ามครอบ layout หรือ page ทั้งหมดด้วย `'use client'`

## Type Organization

```ts
// types/index.ts — รวม interface ทุกตัวในไฟล์เดียว
export interface Room {
  _id: string
  roomId: string
  floor: 1 | 2 | 3 | 4 | 5
  status: 'occupied' | 'vacant'
  deposit: { total: number; remaining: number }
}

export interface Bill { ... }
export interface MeterReading { ... }
export interface GlobalConfig { ... }

// import ที่ไหนก็ได้จากที่เดียว
import type { Room, Bill } from '@/types'
```
