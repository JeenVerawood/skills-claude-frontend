# Env Anti-patterns

## ❌ Mistake 1: process.env ตรงๆ ไม่มี type safety

```ts
// ❌ typo ไม่รู้ ไม่มี error
const url = process.env.DATABSE_URL      // ผิด! ได้ undefined
const key = process.env.JWT_SECERT       // ผิด! ได้ undefined
```

✅ Fix: import จาก `lib/env.ts` เสมอ (ดู `templates/env-schema.md`)

---

## ❌ Mistake 2: Secret ใน NEXT_PUBLIC_

```bash
# ❌ embed ลงใน client bundle ทุกคนเห็น
NEXT_PUBLIC_DATABASE_URL=postgresql://user:pass@host/db
NEXT_PUBLIC_JWT_SECRET=super-secret-key
NEXT_PUBLIC_STRIPE_SECRET_KEY=sk_live_...
```

✅ Fix: secrets ต้องไม่มี NEXT_PUBLIC_ prefix

---

## ❌ Mistake 3: ไม่มี .env.example

```
# ❌ สมาชิกใหม่ในทีมไม่รู้ว่าต้องตั้ง env อะไร
# → clone แล้วรันไม่ได้, debug นาน
```

✅ Fix: มี `.env.example` ใน repo เสมอ (ดู `templates/env-schema.md`)

---

## ❌ Mistake 4: ไม่มี validation ตั้งแต่ start

```ts
// ❌ รู้ว่า env หายตอน production crash แล้ว
const secret = process.env.JWT_SECRET  // undefined — ไม่มี error!
const token = sign(payload, secret)    // 💥 crash runtime
```

✅ Fix: Zod parse ตั้งแต่ start server — crash เร็ว fail fast
