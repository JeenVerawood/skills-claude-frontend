# Template: Layer Folder Structure (Root-level, ไม่มี src/)

> แนะนำสำหรับ: โปรเจกต์ขนาดเล็ก–กลาง (< 20 pages), ทีมเดียว
> Project ตัวอย่าง: apartment-management

## คำสั่ง scaffold (copy และรันได้เลย)

```bash
# Route groups
mkdir -p "app/(auth)/login"
mkdir -p "app/(dashboard)/dashboard"
mkdir -p "app/(dashboard)/rooms"
mkdir -p "app/(dashboard)/bills"
mkdir -p app/api

# Root-level folders (ไม่มี src/)
mkdir -p components
mkdir -p lib
mkdir -p models
mkdir -p types

# Route files
touch app/layout.tsx app/page.tsx app/not-found.tsx
touch "app/(auth)/login/page.tsx"
touch "app/(dashboard)/layout.tsx"
touch "app/(dashboard)/dashboard/page.tsx"

# Lib files
touch lib/auth.ts lib/db.ts lib/env.ts lib/utils.ts lib/mockData.ts

# Types
touch types/index.ts

# Middleware
touch middleware.ts
```

## Full Structure

```
my-app/
├── app/
│   ├── (auth)/
│   │   └── login/page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx
│   │   ├── dashboard/page.tsx
│   │   ├── rooms/page.tsx
│   │   ├── meter/page.tsx
│   │   ├── bills/page.tsx
│   │   ├── bills/[id]/print/page.tsx
│   │   └── config/page.tsx
│   ├── api/
│   │   ├── auth/[...nextauth]/route.ts
│   │   ├── rooms/route.ts
│   │   ├── rooms/[id]/route.ts
│   │   ├── bills/route.ts
│   │   ├── bills/[id]/route.ts
│   │   ├── bills/[id]/status/route.ts
│   │   ├── meter/route.ts
│   │   ├── config/route.ts
│   │   └── seed/route.ts
│   ├── layout.tsx
│   ├── page.tsx
│   └── not-found.tsx
│
├── components/             ← flat, แยกด้วย *Client.tsx suffix
│   ├── *Client.tsx         ← 'use client' components
│   └── *.tsx               ← Server-safe components
│
├── lib/
│   ├── auth.ts
│   ├── db.ts
│   ├── env.ts
│   ├── mockData.ts
│   ├── seed.ts
│   └── utils.ts
│
├── models/                 ← Mongoose models (full-stack เท่านั้น)
│   ├── Room.ts
│   ├── MeterReading.ts
│   ├── Bill.ts
│   └── Config.ts
│
├── types/
│   └── index.ts            ← interfaces ทุกตัวรวมในไฟล์เดียว
│
├── middleware.ts
├── .env
├── .env.example
├── tsconfig.json
├── tailwind.config.ts
└── next.config.js
```

## tsconfig.json (copy ทั้งหมด)

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    },
    "strict": true,
    "moduleResolution": "bundler"
  }
}
```

## types/index.ts (pattern)

```ts
// รวม interface ทุกตัวในไฟล์เดียว — import จากที่เดียวทั้ง project
export interface Room {
  _id: string
  roomId: string
  name: string
  floor: 1 | 2 | 3 | 4 | 5
  rentPrice: number
  status: 'occupied' | 'vacant'
  deposit: { total: number; remaining: number }
  createdAt: string
  updatedAt: string
}

export interface Bill { ... }
export interface MeterReading { ... }
export interface GlobalConfig { ... }
```
