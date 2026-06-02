# Environment Variables Rules

## NEXT_PUBLIC_ Rules

```
NEXT_PUBLIC_*  → embed ลง client bundle ตอน build → ทุกคนเห็นได้
ไม่มี prefix   → server-only → ปลอดภัย

NEXT_PUBLIC_ ได้:  API URL, App name, Feature flags, Stripe publishable key
ไม่มี prefix:      DATABASE_URL, JWT_SECRET, API keys ทุกชนิด, SMTP credentials
```

## .env Files Priority

```
.env.local          → สูงสุด (gitignored, local override)
.env.development    → development เท่านั้น
.env.production     → production เท่านั้น
.env                → ต่ำสุด, default (committed)
```

## กฎสำคัญ

- ห้าม secret ใน `.env` (committed) → ใช้ `.env.local` หรือ CI/CD secrets
- `.env.example` ต้อง commit เสมอ (template ไม่มีค่าจริง)
- import จาก `@lib/env` เสมอ — ห้ามใช้ `process.env` ตรงๆ ในโค้ด

---

## External Service URLs ต้องเป็น env variable เสมอ

ไม่ใช่แค่ secrets — URL ของ external service ก็ต้องเป็น env ด้วย
เพราะ URL เปลี่ยนระหว่าง environment (dev/staging/prod)

❌ hardcode
```ts
href="https://line.me/ti/p/~petpet"
href="https://facebook.com/petpet"
```

✅ env variable
```env
NEXT_PUBLIC_LINE_URL=https://line.me/ti/p/~petpet_official
NEXT_PUBLIC_FB_URL=https://facebook.com/petpet
```

กฎ: ถ้า URL มีชื่อ brand หรือ ID อยู่ → ต้องเป็น env variable
