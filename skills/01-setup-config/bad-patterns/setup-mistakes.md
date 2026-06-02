# Setup Anti-patterns — สิ่งที่ห้ามทำตอนเริ่ม project

## ❌ Mistake 1: ไม่ตั้ง react-hooks/exhaustive-deps เป็น error

```json
// ❌ .eslintrc.json — warn หายง่าย, developer มองข้ามได้
{ "rules": { "react-hooks/exhaustive-deps": "warn" } }

// ✅ ต้องเป็น error — บังคับแก้ก่อน commit
{ "rules": { "react-hooks/exhaustive-deps": "error" } }
```

ถ้าตั้งเป็น warn ตั้งแต่แรก → developer จะเห็น eslint-disable เต็ม codebase ในอีก 2 เดือน

---

## ❌ Mistake 2: ไม่มี Providers wrapper

```tsx
// ❌ ยัด Provider ทั้งหมดใน layout.tsx โดยตรง
// layout.tsx กลายเป็น 'use client' ทั้งไฟล์
export default function RootLayout({ children }) {
  const [queryClient] = useState(() => new QueryClient())
  return (
    <html>
      <body>
        <QueryClientProvider client={queryClient}>
          {children}
        </QueryClientProvider>
      </body>
    </html>
  )
}
```

```tsx
// ✅ แยก providers.tsx ออกมา — layout.tsx เป็น Server Component
// layout.tsx (Server Component)
export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <Providers>{children}</Providers>
      </body>
    </html>
  )
}
```

---

## ❌ Mistake 3: ไม่มี health check endpoint

```
❌ ไม่มี /api/health → Docker health check ไม่ได้
                      → monitoring ไม่รู้ว่า app พัง
```

```ts
// ✅ สร้าง app/api/health/route.ts ตั้งแต่แรก
export async function GET() {
  return Response.json({ status: 'healthy' })
}
```

---

## ❌ Mistake 4: ไม่มี CLAUDE.md

```
❌ ไม่มี CLAUDE.md → Claude Code ไม่รู้ว่า skills อยู่ที่ไหน
                   → ไม่รู้ chaining rules
                   → ต้อง prompt บอก context ทุกครั้ง
```

---

## ❌ Mistake 5: Commit .env ไปโดยไม่ตั้งใจ

```bash
# ❌ .gitignore ขาด .env
# → git add . → commit → push → secret หลุด

# ✅ .gitignore ต้องมีก่อน commit แรกเสมอ
.env
.env.local
.env.*.local
```

---

## ❌ Mistake 6: Import ที่ไม่ได้ใช้ค้างอยู่ในไฟล์

```ts
// ❌ import icons/utilities ที่ถูกลบออกจาก JSX แล้วแต่ไม่ได้ลบ import
import { Heart, MessageCircle, CheckCircle, XCircle } from 'lucide-react'
import { Input } from '@/components/ui/Input'
import { Button } from '@/components/ui/Button'

// ถ้าใช้แค่ Heart กับ MessageCircle ใน JSX
// → ESLint warn: 'CheckCircle' is defined but never used
//               'XCircle' is defined but never used
//               'Input' is defined but never used
//               'Button' is defined but never used
```

✅ Fix: ลบ import ที่ไม่ใช้ออกก่อน commit เสมอ

```ts
// ✅ import เฉพาะที่ใช้จริง
import { Heart, MessageCircle } from 'lucide-react'
```

กฎ: **ก่อน generate โค้ดทุกครั้ง ตรวจว่า import ที่เขียนไว้ถูกใช้จริงใน JSX/TS ด้านล่าง**
ถ้าลบ feature ออก → ลบ import ที่เกี่ยวด้วยทันที

---

## ❌ Mistake 7: ใช้ shadcn/ui CSS Variables ใน globals.css โดยไม่ได้ setup shadcn/ui

```css
/* ❌ error ทันทีตอน npm run dev ถ้าไม่ได้ใช้ shadcn/ui */
@layer base {
  * { @apply border-border; }       /* Syntax error: The `border-border` class does not exist */
  body { @apply bg-background; }    /* Syntax error: The `bg-background` class does not exist */
}
```

**ทำไมผิด:** `border-border`, `bg-background`, `text-foreground` เป็น Tailwind utility classes
ที่ต้องการ CSS variable mapping ใน `tailwind.config.ts` แบบนี้:

```ts
// shadcn/ui init เพิ่มสิ่งนี้ให้อัตโนมัติ — ถ้าไม่มี ใช้ class พวกนั้นไม่ได้
colors: {
  border: 'hsl(var(--border))',
  background: 'hsl(var(--background))',
  foreground: 'hsl(var(--foreground))',
}
```

```css
/* ✅ globals.css สำหรับ project ที่ไม่ใช้ shadcn/ui */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  body {
    @apply bg-white text-gray-900 antialiased;
  }
}
```

**กฎ:** ถ้าไม่ได้รัน `npx shadcn-ui@latest init` → ห้ามใช้ `border-border`, `bg-background`,
`text-foreground` ใน globals.css เด็ดขาด — ใช้ Tailwind built-in classes โดยตรงแทน

---

## ❌ Mistake 8: ไม่มี src/types/api.types.ts ตั้งแต่แรก

```ts
// ❌ แต่ละคนใน team สร้าง response type ของตัวเอง
// → ได้ 5 นิยามของ "API response" ใน 5 ไฟล์

// ✅ สร้าง api.types.ts ตั้งแต่แรก → ทุกคนใช้ร่วมกัน
export type ApiSuccess<T> = { data: T; message?: string }
export type ApiError = { error: string; details?: unknown }
```
