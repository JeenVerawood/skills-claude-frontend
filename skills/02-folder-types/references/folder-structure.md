# Folder Structure มาตรฐาน (Non-negotiable)

```
my-app/
├── app/                        ← Route files เท่านั้น
│   ├── (auth)/
│   │   └── login/page.tsx
│   ├── (dashboard)/
│   │   ├── layout.tsx          ← Shared layout (Sidebar + Topbar)
│   │   ├── dashboard/page.tsx
│   │   ├── rooms/page.tsx
│   │   ├── meter/page.tsx
│   │   ├── bills/page.tsx
│   │   ├── bills/[id]/print/page.tsx
│   │   └── config/page.tsx
│   ├── api/
│   │   └── [resource]/route.ts
│   ├── layout.tsx
│   ├── page.tsx
│   └── not-found.tsx
│
├── components/                 ← Components ทั้งหมด (flat, ไม่แยก ui/features)
│   ├── *Client.tsx             ← Client Components (ต้องมี 'use client' บรรทัดแรก)
│   └── *.tsx                   ← Shared display components (Server-safe)
│
├── lib/                        ← Utilities และ server-side logic
│   ├── auth.ts                 ← NextAuth config
│   ├── db.ts                   ← Database connection
│   ├── env.ts                  ← Zod env validation
│   ├── mockData.ts             ← Static mock data (ถ้าไม่ใช้ DB จริง)
│   ├── seed.ts                 ← Database seeding script
│   └── utils.ts                ← Helper functions (cn, formatCurrency, etc.)
│
├── models/                     ← Mongoose models (full-stack เท่านั้น)
│   ├── Room.ts
│   ├── MeterReading.ts
│   ├── Bill.ts
│   └── Config.ts
│
├── types/
│   └── index.ts                ← interfaces ทุกตัวรวมในไฟล์เดียว
│
├── public/
├── middleware.ts               ← Route protection (NextAuth)
├── .env
├── .env.example
├── tsconfig.json
├── tailwind.config.ts
├── next.config.js
└── CLAUDE.md
```

## กฎเหล็ก

- `app/` → route files เท่านั้น: `page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`, `route.ts`
- ห้าม component / hook / type อยู่ใน `app/` เด็ดขาด
- `components/` อยู่ที่ root ไม่มี `src/` wrapper
- Components แยกด้วย suffix `*Client.tsx` (มี 'use client') vs ไม่มี suffix (Server-safe)
- `types/index.ts` → รวม interface ทุกตัวไว้ไฟล์เดียว (import จากที่เดียว)
- `models/` → เฉพาะ full-stack project ที่ใช้ Mongoose
