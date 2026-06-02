# src/lib/env.ts (copy ได้เลย)

```ts
import { z } from 'zod'

const envSchema = z.object({
  // Server-only — ห้าม NEXT_PUBLIC_
  NODE_ENV:        z.enum(['development', 'test', 'production']),
  DATABASE_URL:    z.string().url().optional(),   // เพิ่มเมื่อเลือก DB
  JWT_SECRET:      z.string().min(32).optional(),
  NEXTAUTH_SECRET: z.string().min(32).optional(),
  NEXTAUTH_URL:    z.string().url().optional(),

  // Client + Server — NEXT_PUBLIC_ ได้
  NEXT_PUBLIC_APP_NAME: z.string().default('My App'),
  NEXT_PUBLIC_APP_URL:  z.string().url().default('http://localhost:3000'),
})

export const env = envSchema.parse(process.env)
export type Env = z.infer<typeof envSchema>
```

## วิธีใช้

```ts
// ✅ import จาก env.ts เสมอ — ห้ามใช้ process.env ตรงๆ
import { env } from '@/lib/env'

const dbUrl = env.DATABASE_URL
const appName = env.NEXT_PUBLIC_APP_NAME
```

## เพิ่ม variable ใหม่

```ts
// 1. เพิ่มใน schema
const envSchema = z.object({
  ...
  SMTP_HOST: z.string().optional(),
  SMTP_PORT: z.coerce.number().optional(),
})

// 2. เพิ่มใน .env.example
// SMTP_HOST=smtp.gmail.com
// SMTP_PORT=587
```

## .env.example (สร้างคู่กัน)

```bash
# === Server Only ===
NODE_ENV=development
# DATABASE_URL=          # เพิ่มเมื่อเลือก DB
# JWT_SECRET=            # openssl rand -base64 32
# NEXTAUTH_SECRET=       # openssl rand -base64 32
# NEXTAUTH_URL=http://localhost:3000

# === Client Safe ===
NEXT_PUBLIC_APP_NAME=My App
NEXT_PUBLIC_APP_URL=http://localhost:3000
```
