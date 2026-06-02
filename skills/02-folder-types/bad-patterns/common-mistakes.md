# Common Structure Mistakes

## ❌ Mistake 1: Component ปนอยู่ใน app/

```
app/
└── dashboard/
    ├── page.tsx
    ├── DashboardCard.tsx   ← ❌ component ปนกับ route
    ├── useDashboard.ts     ← ❌ hook ปนกับ route
    └── dashboard.types.ts  ← ❌ type ปนกับ route
```

**ทำไมผิด:** `app/` directory ถูก scan เป็น route โดย Next.js — component ที่วางไว้จะไม่ถูก tree-shake ถูกต้อง และทำให้ทีมหา file ไม่เจอ

---

## ❌ Mistake 2: Relative import ลึกเกิน 2 ชั้น

```ts
// ❌ อ่านยาก, เปราะ (refactor แล้วพัง)
import { Button } from '../../../../components/ui/Button'
import { useAuth } from '../../../hooks/useAuth'
import type { User } from '../../types/user'
```

---

## ❌ Mistake 3: Type กระจายหลายที่

```
src/
├── components/
│   └── dashboard/
│       └── DashboardCard.types.ts   ← ❌ type อยู่ใน component folder
├── hooks/
│   └── useAuth.types.ts             ← ❌ type อยู่ใน hook folder
└── pages/
    └── user.interface.ts            ← ❌ ชื่อ naming ไม่ consistent
```

**ผลเสีย:** import User ได้หลายที่ → เกิด duplicate type definition → conflict

---

## ❌ Mistake 4: ผสม Layer folder กับ Feature folder

```
src/
├── components/      ← layer style
├── hooks/           ← layer style
└── features/        ← ❌ feature style ปนกัน
    └── auth/
        └── components/
```

---

## ❌ Mistake 5: 'use client' ครอบ layout ใหญ่

```tsx
// app/dashboard/layout.tsx
'use client'  // ❌ ทำให้ทั้ง dashboard tree กลายเป็น client bundle

export default function DashboardLayout({ children }) {
  return <div>{children}</div>  // layout ไม่ได้ต้องการ client เลย!
}
```
