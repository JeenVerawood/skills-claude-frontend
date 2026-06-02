---
name: 09-env-config
description: >
  Use this skill for any question about environment variables, configuration management, .env files,
  type-safe env, NEXT_PUBLIC_ prefix, secrets management, multi-environment setup, and
  next.config.js. Triggers include: "env", "environment variable", "NEXT_PUBLIC", ".env",
  "config", "secret", "process.env", "Zod env", "type-safe env", "env หาย", "env ไม่ถูก",
  "staging", "production config", "next.config". Always use this skill before writing any
  environment variable access or next.config.js changes.
---

# 09-env-config: Environment & Config

## Operating Stance

- **Paranoid about secrets.** ถ้าไม่แน่ใจว่า variable นี้ส่งไป client ได้ไหม → ไม่ส่ง
- **Validate at startup.** env ที่ไม่ valid ต้องทำให้ app crash ตอน start ไม่ใช่ตอน runtime ชั่วโมงที่ 3
- **No `process.env` โดยตรง.** ทุก env access ต้องผ่าน `src/lib/env.ts` ที่ Zod validate แล้วเท่านั้น

---

## When NOT to use

- **ปัญหาเป็น API route** → ใช้ Skill 05 (env.BACKEND_URL ใช้ผ่าน env.ts แล้ว)
- **ปัญหาเป็น deployment** → ใช้ Skill 11
- **แค่ถาม next.config.js เรื่อง image domains** → อ่าน `references/nextconfig.md` แล้วตอบตรง

---

## Refuse to proceed ถ้า

```
❌ user ขอ NEXT_PUBLIC_ สำหรับ secret (API key, token, password) → ปฏิเสธทันที อธิบาย risk
❌ user ขอ access process.env โดยตรงใน component → เสนอ env.ts แทน ห้าม comply
❌ user ขอ commit .env ลง git → ปฏิเสธ อธิบาย และเสนอ .env.example แทน
```

---

## ไฟล์ที่มีและเมื่อไหรควรอ่าน

### references/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `references/env-rules.md` | กฎ NEXT_PUBLIC_, server-only, client-safe |
| `references/nextconfig.md` | next.config.js, security headers, image domains |

### bad-patterns/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `bad-patterns/env-mistakes.md` | typo env, secret หลุด client, process.env ตรงๆ |

### templates/
| ไฟล์ | อ่านเมื่อ |
|---|---|
| `templates/env-schema.md` | Zod env schema + .env.example สำเร็จรูป |

---

## Output Format (บังคับทุกครั้ง)

1. **แยกชัด** ว่า variable ไหน server-only / client-safe พร้อมอธิบายว่าทำไม
2. แสดง Zod env schema ใน `src/lib/env.ts` เสมอ
3. **เตือนทันที** ถ้ามี secret ใน NEXT_PUBLIC_

---

## Worked Example

**คำถาม:** "จะใช้ API key สำหรับ payment gateway ใน Next.js ยังไง?"

**วิธีตอบที่ถูกต้อง:**
```
Payment API key คือ secret → ต้องเป็น server-only เด็ดขาด

❌ อันตรายมาก:
NEXT_PUBLIC_PAYMENT_KEY=sk_live_xxxx  ← expose ไป browser ทุกคน

✅ ถูกต้อง:
// .env.local
PAYMENT_API_KEY=sk_live_xxxx  ← ไม่มี NEXT_PUBLIC_ prefix = server-only

// src/lib/env.ts
import { z } from 'zod'

const serverSchema = z.object({
  PAYMENT_API_KEY: z.string().min(1),
  BACKEND_URL: z.string().url(),
})

const clientSchema = z.object({
  NEXT_PUBLIC_APP_URL: z.string().url(),
})

export const env = {
  ...serverSchema.parse(process.env),      // server-only
  ...clientSchema.parse(process.env),      // client-safe
}

// การใช้งาน — ทุกที่ต้อง import จาก env.ts เท่านั้น
import { env } from '@/lib/env'
const key = env.PAYMENT_API_KEY  // ✅ type-safe, validated at startup
```

---

## Behavioral Rules

```
[NO-NEXT-PUBLIC-SECRET]  ห้าม NEXT_PUBLIC_ สำหรับ secret ทุกกรณี
[ENV-TS-ONLY]            ทุก env access ต้องผ่าน env.ts — ห้าม process.env โดยตรง
[VALIDATE-STARTUP]       Zod schema ต้อง parse ตอน module load — crash early ดีกว่า crash late
[NO-COMMIT-ENV]          ห้าม commit .env — แค่ .env.example ที่ไม่มีค่าจริง
[LOG-UPDATE]       หลังแก้ bug หรือแก้ไข project files → append entry ลง LOG.md ทันที
                   format: Bug Fix entry (trigger, root cause, files modified, fix)
                   ห้าม batch ทีหลัง — บันทึกทันทีหลังแต่ละ action
```

## Cross-skill Chain

```
ก่อนหน้า: start-all-project Phase 3 → สร้าง env.ts + .env.example
ถัดไป:    Skill 05 (API) → env.BACKEND_URL ใช้ใน Route Handlers
          Skill 11 (Deployment) → .env.example เป็น guide สำหรับ production secrets
```
