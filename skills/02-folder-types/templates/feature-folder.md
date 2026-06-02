# Template: Feature Folder Structure (Root-level, ไม่มี src/)

> แนะนำสำหรับ: โปรเจกต์ใหญ่ (20+ pages), ทีมแยกกันทำแต่ละ feature ชัดเจน
> หมายเหตุ: สำหรับ project ขนาดเล็ก–กลาง ให้ใช้ layer-folder.md แทน

## คำสั่ง scaffold

```bash
# Route groups
mkdir -p "app/(auth)/login"
mkdir -p "app/(dashboard)/dashboard"
mkdir -p app/api

# Feature folders (แทน src/features/)
mkdir -p features/auth/components
mkdir -p features/auth/hooks
mkdir -p features/dashboard/components
mkdir -p features/dashboard/hooks

# Shared (แทน src/shared/)
mkdir -p components/ui
mkdir -p lib
mkdir -p types
mkdir -p models

touch middleware.ts
touch types/index.ts
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
│   │   └── [feature]/page.tsx
│   ├── api/
│   ├── layout.tsx
│   └── page.tsx
│
├── features/               ← feature-specific (ไม่ใช้ข้าม feature)
│   ├── auth/
│   │   ├── components/
│   │   │   └── LoginForm.tsx
│   │   ├── hooks/
│   │   │   └── useAuth.ts
│   │   └── index.ts        ← re-export public API ของ feature
│   │
│   └── dashboard/
│       ├── components/
│       │   ├── DashboardClient.tsx
│       │   └── StatCard.tsx
│       ├── hooks/
│       └── index.ts
│
├── components/             ← shared ใช้ได้ทุก feature
│   └── ui/
│       ├── Button.tsx
│       ├── Modal.tsx
│       └── Badge.tsx
│
├── lib/                    ← server-side utilities
│   ├── auth.ts
│   ├── db.ts
│   ├── env.ts
│   └── utils.ts
│
├── models/                 ← Mongoose models (ถ้าใช้ MongoDB)
│
├── types/
│   └── index.ts
│
└── middleware.ts
```

## tsconfig.json

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"],
      "@features/*": ["./features/*"],
      "@ui/*": ["./components/ui/*"]
    }
  }
}
```

## features/auth/index.ts (pattern สำหรับทุก feature)

```ts
// Re-export เฉพาะสิ่งที่ feature อื่นจะใช้
export { LoginForm } from './components/LoginForm'
export { useAuth } from './hooks/useAuth'
export type { AuthUser } from './types'
// ไม่ export implementation details
```

## ข้อควรระวัง

- ห้าม feature import จาก feature อื่นโดยตรง → ต้อง import ผ่าน `components/` หรือ props
- ทุก feature ต้องมี `index.ts` เป็น public API
- ห้ามผสมกับ layer folder style — เลือกอย่างใดอย่างหนึ่ง
- **ไม่มี `src/`** — ทุกอย่างอยู่ที่ root
