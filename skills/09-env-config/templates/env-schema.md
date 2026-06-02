# Template: Type-safe Env (copy ได้เลย)

## src/lib/env.ts

```ts
import { z } from 'zod'

const envSchema = z.object({
  // Server-only — ห้าม NEXT_PUBLIC_
  NODE_ENV:        z.enum(['development', 'test', 'production']),
  DATABASE_URL:    z.string().url(),
  JWT_SECRET:      z.string().min(32),
  NEXTAUTH_SECRET: z.string().min(32),
  NEXTAUTH_URL:    z.string().url(),

  // Client + Server — NEXT_PUBLIC_ ได้
  NEXT_PUBLIC_API_URL:  z.string().url(),
  NEXT_PUBLIC_APP_NAME: z.string().default('My App'),

  // Optional
  SMTP_HOST:  z.string().optional(),
  REDIS_URL:  z.string().url().optional(),
})

export const env = envSchema.parse(process.env)
// crash ตั้งแต่ start server ถ้า env ไม่ครบ/typo
```

## .env.example (commit เข้า git)

```bash
# === Server Only (ห้าม NEXT_PUBLIC_) ===
NODE_ENV=development
DATABASE_URL=postgresql://USER:PASSWORD@localhost:5432/DB_NAME
JWT_SECRET=generate-with-openssl-rand-base64-32
NEXTAUTH_SECRET=generate-with-openssl-rand-base64-32
NEXTAUTH_URL=http://localhost:3000

# === Client Safe (NEXT_PUBLIC_ ได้) ===
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_APP_NAME=My App

# === Optional ===
# SMTP_HOST=smtp.gmail.com
# REDIS_URL=redis://localhost:6379
```

## .gitignore (ต้องมี)

```
.env.local
.env.*.local
```

## การ generate secret

```bash
# JWT_SECRET / NEXTAUTH_SECRET
openssl rand -base64 32
```
